# Chapter 24 — Basic Signal and Data Processing

## 24.1 Introduction

Scientific instruments often produce a sequence of measurements rather than a single value.

Examples include:

- voltage measured over time
- current measured over time
- temperature measured during an experiment
- photodiode output measured under changing illumination
- sensor output recorded at regular time intervals

Such measurements form a **signal** or a **data sequence**.

Before scientific analysis, we may need to:

- inspect the signal
- visualize it
- calculate basic statistics
- identify unusual values
- smooth small fluctuations
- detect peaks
- calculate rates of change

Python, NumPy, Matplotlib, and SciPy provide useful tools for these tasks.

The basic workflow is:

$$
\boxed{
\text{Measured Signal}
\rightarrow
\text{Inspect}
\rightarrow
\text{Visualize}
\rightarrow
\text{Process}
\rightarrow
\text{Analyze}
}
$$

---

## 24.2 What Is a Signal?

A signal is a quantity that changes with an independent variable.

For example, current measured over time can be written as:

$$
I=I(t)
$$

where:

- \(I\) = current
- \(t\) = time

A voltage signal can be written as:

$$
V=V(t)
$$

Semiconductor measurements can also be written as:

$$
I=I(V)
$$

where current is measured as voltage is changed.

Therefore, scientific data can be treated as a signal even when the independent variable is not time.

---

## 24.3 A Small Experimental Signal

Suppose a sensor produces the following voltage measurements:

| Time (s) | Voltage (V) |
|---:|---:|
| 0 | 1.00 |
| 1 | 1.12 |
| 2 | 1.25 |
| 3 | 1.18 |
| 4 | 1.35 |
| 5 | 1.42 |
| 6 | 1.38 |
| 7 | 1.55 |

We can store the data using NumPy.

```python
import numpy as np

t = np.array([0, 1, 2, 3, 4, 5, 6, 7])
V = np.array([1.00, 1.12, 1.25, 1.18, 1.35, 1.42, 1.38, 1.55])
```

Here:

```text
t → independent variable
V → measured signal
```

---

## 24.4 First Step — Visualize the Signal

Before applying any processing method, plot the data.

```python
import matplotlib.pyplot as plt

plt.plot(t, V, marker="o")
plt.xlabel("Time (s)")
plt.ylabel("Voltage (V)")
plt.title("Measured Voltage Signal")
plt.grid(True)
plt.show()
```

The resulting plot is included below.

```{figure} ../images/ch24/ch24_raw_voltage_signal.png
:name: ch24-raw-voltage-signal
:alt: Plot of measured voltage against time
:align: center

Measured voltage signal.
```


A plot immediately helps us see:

- the general trend
- fluctuations
- unusually large changes
- possible measurement problems

### Important principle

> Always look at scientific data before processing it.

---

## 24.5 Basic Signal Statistics

We can calculate simple statistics using NumPy.

### Mean

```python
mean_V = np.mean(V)
print(mean_V)
```

The mean represents the average measured voltage.

### Standard deviation

```python
std_V = np.std(V)
print(std_V)
```

The standard deviation gives an indication of the spread of the measurements.

### Minimum and maximum

```python
print("Minimum =", np.min(V))
print("Maximum =", np.max(V))
```

These values help describe the range of the signal.

---

## 24.6 Detecting Sudden Changes

Suppose a signal is approximately stable but suddenly changes.

We can calculate the difference between consecutive measurements.

```python
difference = np.diff(V)

print(difference)
```

Mathematically,

$$
\Delta V_i=V_{i+1}-V_i
$$

A large value of \(\Delta V\) indicates a relatively large change between two consecutive observations.

This can be useful when examining:

- sensor signals
- device switching
- measurement disturbances
- sudden changes in experimental conditions

---

## 24.7 Numerical Differentiation of a Signal

If a signal changes with time, its rate of change can be estimated numerically.

For voltage:

$$
\frac{dV}{dt}
$$

represents the rate at which voltage changes with time.

NumPy provides `gradient()` for numerical differentiation.

```python
dV_dt = np.gradient(V, t)

print(dV_dt)
```

We can plot the result.

```python
plt.plot(t, dV_dt, marker="o")
plt.xlabel("Time (s)")
plt.ylabel("dV/dt (V/s)")
plt.title("Rate of Change of Voltage")
plt.grid(True)
plt.show()
```

```{figure} ../images/ch24/ch24_rate_of_change.png
:name: ch24-rate-of-change
:alt: Rate of change of voltage with time
:align: center

Numerical rate of change of the measured voltage.
```

### Scientific interpretation

A large positive value means that voltage is increasing rapidly.

A large negative value means that voltage is decreasing rapidly.

A value close to zero means that the signal is changing slowly.

---

## 24.8 Why Experimental Signals Contain Noise

Real measurements are rarely perfectly smooth.

A simple representation is:

$$
V_{\text{measured}}(t)
=
V_{\text{true}}(t)
+
\text{noise}
$$

Noise can come from:

- electronic components
- measurement instruments
- environmental conditions
- electromagnetic interference
- temperature fluctuations
- limitations of the measurement system

Therefore, a measured signal may contain small fluctuations that are not part of the physical trend.

---

## 24.9 Simple Moving Average

One simple way to reduce small fluctuations is a **moving average**.

For three observations:

$$
V_{\text{smooth},i}
=
\frac{V_{i-1}+V_i+V_{i+1}}{3}
$$

In Python, NumPy's `convolve()` can be used.

```python
window = 3

kernel = np.ones(window) / window

V_smooth = np.convolve(V, kernel, mode="valid")
t_smooth = t[1:-1]
```

Then plot the original and smoothed signals.

```python
plt.plot(t, V, marker="o", label="Measured")
plt.plot(t_smooth, V_smooth, marker="o",
         label="3-point moving average")

plt.xlabel("Time (s)")
plt.ylabel("Voltage (V)")
plt.title("Measured and Smoothed Voltage Signal")
plt.legend()
plt.grid(True)
plt.show()
```

```{figure} ../images/ch24/ch24_moving_average.png
:name: ch24-moving-average
:alt: Original voltage signal compared with a three-point moving average
:align: center

Original measured voltage and three-point moving average.
```

The smoothed signal makes the overall pattern easier to see.

---

## 24.10 What Does Smoothing Do?

Smoothing reduces small fluctuations and makes the general pattern easier to see.

However, smoothing can also remove information.

For example, a very short physical event may be reduced or hidden by excessive smoothing.

Therefore:

$$
\boxed{
\text{Smoothing should clarify data, not hide data}
}
$$

The original measurements should always be retained.

---

## 24.11 Choosing the Moving-Average Window

Consider:

```python
window = 3
```

```python
window = 5
```

```python
window = 7
```

A larger window generally produces stronger smoothing.

However, a larger window also removes more short-term variation.

Therefore:

$$
\boxed{
\text{Larger Window}
\rightarrow
\text{More Smoothing}
\rightarrow
\text{Less Short-Term Detail}
}
$$

The window should be chosen according to the scientific purpose of the analysis.

---

## 24.12 SciPy for Signal Processing

SciPy contains a dedicated signal-processing module:

```python
from scipy import signal
```

It provides functions for tasks such as:

- filtering
- peak detection
- signal analysis
- frequency-domain analysis
- digital signal processing

For this course, the important idea is:

> SciPy provides scientific signal-processing methods that extend the basic operations available in NumPy.

---

## 24.13 Detecting Peaks

A peak is a point where the signal is locally higher than its neighboring values.

SciPy provides `find_peaks()`.

```python
from scipy.signal import find_peaks

peaks, properties = find_peaks(V)

print("Peak positions:", peaks)
print("Peak values:", V[peaks])
```

The returned `peaks` values are array positions.

For example:

```python
print(t[peaks])
```

gives the corresponding time values.

---

## 24.14 Why Peak Detection Is Useful

Peak detection can be useful in:

- sensor signals
- pulse measurements
- device response data
- repeated experimental events
- photodiode measurements

The numerical result should always be interpreted using the experimental setup.

---

## 24.15 Plotting Detected Peaks

We can highlight detected peaks on the original signal.

```python
plt.plot(t, V, marker="o", label="Signal")
plt.plot(t[peaks], V[peaks], marker="o",
         linestyle="None", label="Detected peaks")

plt.xlabel("Time (s)")
plt.ylabel("Voltage (V)")
plt.title("Peak Detection")
plt.legend()
plt.grid(True)
plt.show()
```

```{figure} ../images/ch24/ch24_peak_detection.png
:name: ch24-peak-detection
:alt: Voltage signal with detected peaks highlighted
:align: center

Detected peaks in the measured voltage signal.
```

This combines:

$$
\boxed{
\text{Numerical Analysis}
+
\text{Visualization}
}
$$

The plot helps us verify whether the detected peaks make scientific sense.

---

## 24.16 Basic Filtering — Concept

Filtering means modifying a signal to reduce or isolate certain components.

For example:

- low-pass filtering can reduce rapid fluctuations
- high-pass filtering can emphasize rapid changes
- band-pass filtering can isolate a selected frequency range

Conceptually:

$$
\text{Measured Signal}
=
\text{Useful Signal}
+
\text{Unwanted Components}
$$

A filter attempts to separate these components.

Filtering is more advanced than a simple moving average and should be applied with care.

---

## 24.17 Signal Processing for Semiconductor Experiments

Signal processing can appear in many semiconductor and electronics experiments.

### Example 1 — Sensor voltage

A sensor produces:

$$
V=V(t)
$$

We can:

- plot voltage against time
- calculate mean and standard deviation
- identify peaks
- calculate rate of change
- smooth small fluctuations

### Example 2 — Photodiode response

A photodiode output may be measured while illumination changes.

We can analyze:

$$
I=I(t)
$$

or

$$
I=I(P)
$$

where \(P\) represents optical power.

### Example 3 — Device switching

A voltage or current signal may change rapidly when an electronic device switches.

Numerical differentiation can help identify rapid transitions.

---

## 24.18 A Complete Basic Signal Analysis

Consider the following current-time data.

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import find_peaks

t = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

I = np.array([
    0.10, 0.14, 0.20, 0.17, 0.28,
    0.35, 0.30, 0.42, 0.38, 0.50
])
```

### Step 1 — Plot the signal

```python
plt.plot(t, I, marker="o")
plt.xlabel("Time (s)")
plt.ylabel("Current (A)")
plt.title("Current-Time Signal")
plt.grid(True)
plt.show()
```

### Step 2 — Calculate basic statistics

```python
print("Mean =", np.mean(I))
print("Standard deviation =", np.std(I))
print("Minimum =", np.min(I))
print("Maximum =", np.max(I))
```

### Step 3 — Calculate changes

```python
dI = np.diff(I)
print("Consecutive changes =", dI)
```

### Step 4 — Calculate rate of change

```python
dI_dt = np.gradient(I, t)
print("dI/dt =", dI_dt)
```

### Step 5 — Detect peaks

```python
peaks, _ = find_peaks(I)

print("Peak times =", t[peaks])
print("Peak currents =", I[peaks])
```

### Step 6 — Plot the detected peaks

```python
plt.plot(t, I, marker="o", label="Current")
plt.plot(t[peaks], I[peaks], marker="o",
         linestyle="None", label="Peaks")

plt.xlabel("Time (s)")
plt.ylabel("Current (A)")
plt.title("Current-Time Signal and Detected Peaks")
plt.legend()
plt.grid(True)
plt.show()
```

```{figure} ../images/ch24/ch24_current_time_peaks.png
:name: ch24-current-time-peaks
:alt: Current-time signal with detected peaks
:align: center

Current-time signal with detected peaks.
```

This is a complete basic signal-processing workflow.

---

## 24.19 Important Scientific Caution

A numerical method does not automatically produce a scientifically correct conclusion.

For example, a peak detected by Python may be:

- a real physical event
- measurement noise
- an instrument artifact
- an unexpected experimental condition

Therefore:

$$
\boxed{
\text{Python Result}
\neq
\text{Automatic Scientific Conclusion}
}
$$

The result must be interpreted using:

- the experiment
- the instrument
- the physical model
- the measurement conditions
- the quality of the data

---

## 24.20 Original Data Should Be Preserved

When processing experimental data, keep the original measurements.

For example:

```python
V_original = V.copy()
```

Then perform processing on another variable:

```python
V_processed = V_smooth
```

This allows us to compare:

```text
Original data
        ↓
Processing
        ↓
Processed data
```

Never overwrite valuable raw experimental measurements unnecessarily.

---

## 24.21 Common Mistakes

### Mistake 1 — Smoothing before inspecting the raw data

Always examine the original signal first.

### Mistake 2 — Using excessive smoothing

Too much smoothing can remove real scientific information.

### Mistake 3 — Treating every peak as a physical event

A detected peak may be caused by noise.

### Mistake 4 — Ignoring units

For example:

$$
\frac{dV}{dt}
$$

has units of volts per second when \(V\) is measured in volts and \(t\) in seconds.

### Mistake 5 — Replacing the original data

Always retain the raw measurements.

---

## 24.22 Key Points

- A scientific signal is a sequence of measurements.
- Signals can be functions of time or another experimental variable.
- Visualization should be the first step in signal analysis.
- NumPy can calculate statistics, differences, and numerical derivatives.
- `np.gradient()` can estimate the rate of change.
- A moving average can reduce small fluctuations.
- SciPy provides more advanced signal-processing tools.
- `find_peaks()` can identify local peaks.
- Processing should not remove scientifically meaningful information.
- Original experimental data should always be preserved.
- Numerical results must be interpreted using scientific knowledge.

---

## 24.23 Quick Practice

### Question 1

What is meant by a scientific signal?

### Question 2

What does the following calculate?

```python
np.diff(V)
```

### Question 3

What does `np.gradient(V, t)` represent when \(V\) is voltage and \(t\) is time?

### Question 4

Why should the raw experimental signal be plotted before smoothing?

### Question 5

What is the purpose of `find_peaks()`?

---

## 24.24 Hands-on Activity

Create a Python program for the following experimental current signal:

```python
t = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

I = np.array([
    0.10, 0.13, 0.18, 0.16, 0.25,
    0.31, 0.29, 0.40, 0.37, 0.48
])
```

Perform the following steps:

1. Plot current against time.
2. Calculate the mean current.
3. Calculate the standard deviation.
4. Find the minimum and maximum current.
5. Calculate consecutive differences using `np.diff()`.
6. Calculate \(dI/dt\) using `np.gradient()`.
7. Detect peaks using `find_peaks()`.
8. Plot the detected peaks.
9. Apply a three-point moving average.
10. Compare the original and smoothed signals.

Finally, write two or three sentences explaining what you observe.

---

## 24.25 Chapter Summary

Scientific measurements often produce signals rather than isolated numbers.

Python allows us to inspect, visualize, and process these signals using NumPy and SciPy.

The basic workflow is:

$$
\boxed{
\text{Raw Signal}
\rightarrow
\text{Visualize}
\rightarrow
\text{Calculate}
\rightarrow
\text{Process}
\rightarrow
\text{Verify}
\rightarrow
\text{Interpret}
}
$$

The important principle is to keep the processing connected to the physical experiment.

Python performs the calculations, but the scientist must decide what the results mean.
