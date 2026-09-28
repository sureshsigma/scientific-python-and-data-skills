# Chapter 25 — Diode and Zener Diode Data Analysis

## 25.1 Introduction

A semiconductor device experiment often produces a table of voltage and current measurements.

For a diode, we commonly study the relationship between:

$$
I=I(V)
$$

where:

- \(V\) = applied voltage
- \(I\) = measured current

The main scientific task is not simply to store the numbers.

We want to understand the **shape of the device characteristic** and extract useful information from it.

In this chapter, we will use Python to:

- organize diode data
- plot an I–V characteristic
- identify the forward conduction region
- examine current on a logarithmic scale
- calculate simple quantities from the data
- analyze a Zener diode characteristic
- identify the approximate breakdown region
- compare the numerical results with physical expectations

The basic workflow is:

$$
\boxed{
\text{Experimental Data}
\rightarrow
\text{Clean}
\rightarrow
\text{Plot}
\rightarrow
\text{Identify Region}
\rightarrow
\text{Calculate}
\rightarrow
\text{Interpret}
}
$$

---

## 25.2 What Is a Diode I–V Characteristic?

A diode I–V characteristic shows how current changes when the applied voltage changes.

We can write:

$$
I=f(V)
$$

In a real experiment, we measure several pairs:

$$
(V_1,I_1),(V_2,I_2),\ldots,(V_n,I_n)
$$

These measurements can be stored in a CSV file or directly in NumPy arrays.

---

## 25.3 Example Experimental Data

For classroom practice, consider the following forward-bias measurements.

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.00 | 0.00 |
| 0.05 | 0.00 |
| 0.10 | 0.01 |
| 0.15 | 0.02 |
| 0.20 | 0.03 |
| 0.25 | 0.05 |
| 0.30 | 0.09 |
| 0.35 | 0.16 |
| 0.40 | 0.30 |
| 0.45 | 0.58 |
| 0.50 | 1.10 |
| 0.55 | 2.00 |
| 0.60 | 3.50 |
| 0.65 | 5.80 |
| 0.70 | 9.00 |

This is a **synthetic teaching dataset** designed to resemble a diode forward characteristic.

For an actual laboratory experiment, the measured values should be used instead.

---

## 25.4 Entering the Data in Python

```python
import numpy as np

V = np.array([
    0.00, 0.05, 0.10, 0.15, 0.20,
    0.25, 0.30, 0.35, 0.40, 0.45,
    0.50, 0.55, 0.60, 0.65, 0.70
])

I = np.array([
    0.00, 0.00, 0.01, 0.02, 0.03,
    0.05, 0.09, 0.16, 0.30, 0.58,
    1.10, 2.00, 3.50, 5.80, 9.00
])
```

Here, current is measured in mA.

---

## 25.5 First Step — Plot the I–V Curve

Always visualize experimental data before performing calculations.

```python
import matplotlib.pyplot as plt

plt.plot(V, I, marker="o")
plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Diode I–V Characteristics")
plt.grid(True)
plt.show()
```

```{figure} ../images/ch25/ch25_diode_iv.png
:name: ch25-diode-iv
:alt: Diode forward current versus voltage
:align: center

Diode forward I–V characteristic.
```

The curve shows an important feature:

> Current remains small at lower voltage and increases rapidly at higher forward voltage.

The exact shape depends on the diode, temperature, measurement setup, and other experimental conditions.

---

## 25.6 Identifying the Forward-Conduction Region

The I–V curve can be divided conceptually into regions.

### Low-voltage region

Current is relatively small.

### Forward-conduction region

Current begins to increase significantly as voltage increases.

### High-current region

A small increase in voltage can produce a comparatively large increase in current.

The exact transition should not be treated as one universal voltage for every diode.

It depends on the device and experimental conditions.

---

## 25.7 Finding a Current at a Given Voltage

Suppose we want to know the measured current at:

$$
V=0.50\text{ V}
$$

From the dataset:

$$
I=1.10\text{ mA}
$$

In Python, we can locate the measurement.

```python
index = np.where(V == 0.50)[0]

print(I[index])
```

Output:

```text
[1.1]
```

Therefore:

$$
\boxed{I=1.10\text{ mA}}
$$

This is a **measured data point**, not an interpolation.

---

## 25.8 Finding the Maximum Current

NumPy can find the maximum measured current.

```python
max_I = np.max(I)

print("Maximum current =", max_I, "mA")
```

We can also find its position.

```python
index = np.argmax(I)

print("Voltage =", V[index], "V")
print("Current =", I[index], "mA")
```

For this dataset:

$$
I_{\max}=9.00\text{ mA}
$$

at

$$
V=0.70\text{ V}
$$

---

## 25.9 Current Change Between Two Measurements

Suppose current changes from:

$$
I_1=0.30\text{ mA}
$$

to

$$
I_2=0.58\text{ mA}
$$

The change is:

$$
\Delta I=I_2-I_1
$$

Therefore:

$$
\Delta I=0.58-0.30=0.28\text{ mA}
$$

In Python:

```python
difference = np.diff(I)

print(difference)
```

This calculates the change between every pair of consecutive current measurements.

---

## 25.10 Estimating the Slope of the I–V Curve

The slope of an I–V curve represents how rapidly current changes with voltage.

Approximately,

$$
\frac{\Delta I}{\Delta V}
$$

For two neighboring measurements:

$$
\frac{\Delta I}{\Delta V}
=
\frac{I_2-I_1}{V_2-V_1}
$$

For example, between:

$$
V_1=0.50\text{ V},\quad I_1=1.10\text{ mA}
$$

and

$$
V_2=0.55\text{ V},\quad I_2=2.00\text{ mA}
$$

we obtain:

$$
\frac{\Delta I}{\Delta V}
=
\frac{2.00-1.10}{0.55-0.50}
$$

so:

$$
\frac{\Delta I}{\Delta V}
=
18\text{ mA/V}
$$

A large slope means current is changing rapidly with voltage.

---

## 25.11 Differential Resistance

The inverse slope of an I–V characteristic is related to **differential resistance**.

Approximately:

$$
r_d=\frac{\Delta V}{\Delta I}
$$

Using the previous interval:

$$
r_d
=
\frac{0.55-0.50}{2.00-1.10}
$$

with current converted from mA to A.

Since:

$$
0.90\text{ mA}=0.00090\text{ A}
$$

we obtain:

$$
r_d
=
\frac{0.05}{0.00090}
\approx55.6\ \Omega
$$

Therefore, the approximate differential resistance over this interval is:

$$
\boxed{r_d\approx55.6\ \Omega}
$$

This is an interval estimate, not the exact resistance at a single point.

---

## 25.12 Using `np.gradient()` for the Slope

Instead of manually calculating every interval, NumPy can estimate the derivative.

```python
dI_dV = np.gradient(I, V)

print(dI_dV)
```

Here:

$$
\frac{dI}{dV}
$$

represents the approximate local slope of the I–V curve.

The corresponding differential resistance can be estimated as:

$$
r_d\approx\frac{1}{dI/dV}
$$

provided the units are handled correctly.

---

## 25.13 Why Use a Logarithmic Current Scale?

Diode current can change by a large amount over a relatively small voltage range.

A logarithmic y-axis can make this behavior easier to examine.

```python
positive = I > 0

plt.semilogy(V[positive], I[positive], marker="o")
plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA, log scale)")
plt.title("Diode Forward Current on a Log Scale")
plt.grid(True)
plt.show()
```

```{figure} ../images/ch25/ch25_diode_semilog.png
:name: ch25-diode-semilog
:alt: Diode forward current plotted using a logarithmic current axis
:align: center

Diode forward current shown on a logarithmic current scale.
```

### Why does this help?

A normal plot emphasizes absolute differences.

A logarithmic plot makes multiplicative changes easier to see.

For diode data, this can help us examine the rapid increase in current more clearly.

---

## 25.14 The Diode Equation

An idealized diode relationship is often represented by:

$$
I=I_s
\left(
e^{V/(nV_T)}-1
\right)
$$

where:

- \(I_s\) = saturation current
- \(n\) = ideality factor
- \(V_T\) = thermal voltage
- \(V\) = applied voltage

The equation provides a physical model for the device.

However, experimental data may not follow the equation exactly because of:

- temperature
- measurement error
- series resistance
- device construction
- leakage effects
- limitations of the idealized model

Therefore, a measured I–V curve should be interpreted as experimental evidence rather than as a perfect theoretical curve.

---

# Part B — Zener Diode

## 25.15 What Is a Zener Diode?

A Zener diode is designed to operate in a reverse-bias region where the current increases significantly after a characteristic breakdown region is reached.

A simplified characteristic contains:

- a forward-bias region
- a reverse-leakage region
- a breakdown region

The breakdown voltage is commonly denoted by:

$$
V_Z
$$

---

## 25.16 Example Zener Data

Consider the following synthetic teaching data.

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.0 | 0.00 |
| 1.0 | 0.00 |
| 2.0 | 0.00 |
| 3.0 | 0.00 |
| 4.0 | -0.01 |
| 5.0 | -0.02 |
| 5.5 | -0.04 |
| 5.8 | -0.10 |
| 6.0 | -0.25 |
| 6.2 | -0.60 |
| 6.4 | -1.20 |
| 6.6 | -2.20 |
| 6.8 | -3.80 |
| 7.0 | -5.50 |

Negative current represents the chosen current-direction convention for reverse bias.

---

## 25.17 Plotting the Zener Characteristic

```python
Vz = np.array([
    0, 1, 2, 3, 4, 5, 5.5,
    5.8, 6.0, 6.2, 6.4, 6.6,
    6.8, 7.0
])

Iz = np.array([
    0, 0, 0, 0, -0.01, -0.02, -0.04,
    -0.10, -0.25, -0.60, -1.20, -2.20,
    -3.80, -5.50
])

plt.plot(Vz, Iz, marker="o")
plt.axhline(0, linewidth=0.8)
plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Zener Diode I–V Characteristics")
plt.grid(True)
plt.show()
```

```{figure} ../images/ch25/ch25_zener_iv.png
:name: ch25-zener-iv
:alt: Zener diode current versus voltage
:align: center

Zener diode I–V characteristic showing the reverse-bias region.
```

The graph shows that the reverse current becomes increasingly large in magnitude as the voltage approaches and passes through the breakdown region.

---

## 25.18 Estimating the Breakdown Region

There is not always one perfectly sharp point in experimental data.

Instead, we identify a region where the reverse current begins increasing rapidly.

For this teaching dataset, the change becomes pronounced around:

$$
V\approx6.0\text{--}6.4\text{ V}
$$

This is an **approximate experimental interpretation**.

The actual breakdown voltage of a particular Zener diode should be obtained from its measured characteristic and device specifications.

---

## 25.19 Finding a Threshold from Data

Suppose we define a practical current threshold:

$$
|I_Z|\geq1\text{ mA}
$$

We can find the first measurement satisfying this condition.

```python
condition = np.abs(Iz) >= 1.0

index = np.where(condition)[0][0]

print("Voltage =", Vz[index], "V")
print("Current =", Iz[index], "mA")
```

For this dataset, the first point satisfying the condition is approximately:

$$
V=6.4\text{ V}
$$

This illustrates an important idea:

> A numerical threshold can be used to define an operational point from experimental data.

The chosen threshold must be scientifically justified.

---

## 25.20 Comparing Diode and Zener Data

A normal diode is commonly studied in forward bias.

A Zener diode is commonly studied for its reverse-bias breakdown behavior.

Conceptually:

$$
\boxed{
\text{Diode}
\rightarrow
\text{Forward Conduction}
}
$$

and

$$
\boxed{
\text{Zener Diode}
\rightarrow
\text{Reverse Breakdown Region}
}
$$

The exact characteristics depend on the devices and experimental conditions.

---

## 25.21 Combining Device Data

We can plot both datasets for comparison.

```python
plt.plot(V, I_diode_mA, marker="o", label="Forward diode")
plt.plot(Vz, Iz_mA, marker="o", label="Zener reverse region")

plt.axhline(0, linewidth=0.8)
plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Diode and Zener Device Characteristics")
plt.legend()
plt.grid(True)
plt.show()
```

```{figure} ../images/ch25/ch25_diode_zener_comparison.png
:name: ch25-diode-zener-comparison
:alt: Comparison of diode forward characteristic and Zener reverse characteristic
:align: center

Comparison of diode and Zener characteristics using the teaching datasets.
```

The comparison helps students connect the numerical data with the physical behavior of the devices.

---

## 25.22 Scientific Interpretation

When analyzing a device characteristic, ask:

1. What is the independent variable?
2. What is the measured response?
3. What is the overall trend?
4. Where does the response change rapidly?
5. Are there unusual measurements?
6. What physical region does the observed behavior represent?
7. Are the units correct?
8. Is the conclusion supported by the experimental data?

This turns a Python plot into a scientific analysis.

---

## 25.23 Experimental Data vs Synthetic Data

The diode and Zener datasets in this chapter are **synthetic teaching datasets**.

They are useful because:

- the pattern is easy to understand
- students can reproduce the calculations
- the dataset is small
- the expected behavior is clear

In a laboratory exercise, students should replace these arrays with actual measurements.

For example:

```python
import pandas as pd

data = pd.read_csv("diode_iv.csv")

V = data["Voltage_V"].to_numpy()
I = data["Current_mA"].to_numpy()
```

The analysis code can then remain largely the same.

---

## 25.24 Common Mistakes

### Mistake 1 — Plotting without units

Always label:

```text
Voltage (V)
Current (mA)
```

rather than simply:

```text
Voltage
Current
```

### Mistake 2 — Confusing mA and A

Remember:

$$
1\text{ mA}=10^{-3}\text{ A}
$$

### Mistake 3 — Calling every rapid change "breakdown"

A breakdown region should be identified from the measured characteristic and the experimental context.

### Mistake 4 — Treating synthetic data as experimental data

Always clearly identify teaching data as synthetic.

### Mistake 5 — Ignoring current sign

The sign depends on the selected current-direction convention.

### Mistake 6 — Using a threshold without explanation

If a threshold is used to define a device characteristic, explain why that threshold was chosen.

---

## 25.25 Key Points

- A diode I–V characteristic describes current as a function of voltage.
- Experimental device data should be visualized before detailed analysis.
- `np.max()` and `np.argmax()` can identify maximum measurements.
- `np.diff()` can calculate changes between consecutive measurements.
- `np.gradient()` can estimate the local slope.
- Differential resistance is related to the inverse slope of the I–V curve.
- A logarithmic current scale can help visualize rapidly changing diode current.
- A Zener diode shows characteristic reverse-bias breakdown behavior.
- Experimental breakdown is better viewed as a region than as an automatically exact single point.
- Units and sign conventions must be handled carefully.
- Synthetic teaching data and real experimental data should be clearly distinguished.

---

## 25.26 Quick Practice

### Question 1

What does an I–V characteristic show?

### Question 2

What does the slope

$$
\frac{\Delta I}{\Delta V}
$$

represent physically?

### Question 3

Why can a logarithmic current axis be useful for diode data?

### Question 4

What is meant by the Zener breakdown region?

### Question 5

Why should mA be converted to A when calculating resistance in ohms?

---

## 25.27 Hands-on Activity

Use the following diode data:

```python
V = np.array([
    0.00, 0.10, 0.20, 0.30, 0.40,
    0.45, 0.50, 0.55, 0.60, 0.65, 0.70
])

I = np.array([
    0.00, 0.01, 0.03, 0.08, 0.30,
    0.58, 1.10, 2.00, 3.50, 5.80, 9.00
])
```

Perform the following:

1. Plot the I–V characteristic.
2. Label both axes with units.
3. Find the maximum current.
4. Find the voltage corresponding to the maximum current.
5. Calculate consecutive current differences.
6. Estimate \(dI/dV\) using `np.gradient()`.
7. Plot the data using a logarithmic y-axis.
8. Identify the region where current begins increasing rapidly.
9. Write two or three sentences interpreting the curve.

### Extension

Repeat the analysis using actual diode laboratory measurements if available.

---

## 25.28 Chapter Summary

Device data analysis connects experimental measurements with semiconductor behavior.

For diode data, the central relationship is:

$$
I=I(V)
$$

Python allows us to move from raw measurements to a scientific interpretation:

$$
\boxed{
\text{Voltage-Current Data}
\rightarrow
\text{I–V Plot}
\rightarrow
\text{Characteristic Regions}
\rightarrow
\text{Numerical Analysis}
\rightarrow
\text{Physical Interpretation}
}
$$

For Zener data, the reverse-bias characteristic can be used to identify the approximate breakdown region.

The key principle is:

> **Do not stop at the plot. Use the plot and numerical calculations to explain what the device is doing.**
