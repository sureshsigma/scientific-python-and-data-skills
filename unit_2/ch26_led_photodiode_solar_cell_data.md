# Chapter 26 — LED, Photodiode and Solar Cell Data

## 26.1 Introduction

Different semiconductor devices produce different electrical responses.

In this chapter, we use Python to analyze data from:

- LED
- photodiode
- solar cell

The main idea is:

**Device → Experimental Data → Python → Plot → Calculate → Interpret**

The datasets used in the examples are **synthetic teaching datasets** designed to represent typical device behaviour. In a real laboratory, the same analysis can be applied to measured data.

---

## 26.2 LED Characteristics

An LED is a semiconductor diode that emits light when forward biased.

Its important electrical relationship is the forward current as a function of forward voltage:

$$
I=f(V)
$$

For an LED, current is usually very small at lower forward voltages and increases rapidly after the characteristic knee region.

### Example Dataset

```python
import numpy as np

V = np.array([1.60, 1.70, 1.75, 1.80, 1.85,
              1.90, 1.95, 2.00, 2.05, 2.10])

I_mA = np.array([0.00, 0.01, 0.04, 0.12, 0.35,
                 0.85, 1.80, 3.50, 6.20, 10.0])
```

Here:

- `V` = forward voltage in volts
- `I_mA` = forward current in milliamperes

---

## 26.3 Plotting the LED I–V Curve

```python
import matplotlib.pyplot as plt

plt.plot(V, I_mA, marker="o")
plt.xlabel("Forward Voltage (V)")
plt.ylabel("Forward Current (mA)")
plt.title("LED Forward I–V Characteristic")
plt.grid()
plt.show()
```

```{figure} ../images/ch26/ch26_led_iv.png
:name: ch26-led-iv
:align: center

LED forward I–V characteristic.
```

### Interpretation

The graph shows that:

1. Current is very small at lower voltage.
2. Current starts increasing more noticeably in the knee region.
3. After this region, a small increase in voltage produces a much larger increase in current.

The exact curve depends on the LED material, temperature, device construction, and measurement conditions.

---

## 26.4 Finding the Maximum Measured Current

```python
max_current = np.max(I_mA)

print("Maximum current:", max_current, "mA")
```

The maximum measured current in this dataset is:

$$
I_{\max}=10.0\text{ mA}
$$

We can also find the corresponding voltage.

```python
index = np.argmax(I_mA)

print("Voltage:", V[index], "V")
print("Current:", I_mA[index], "mA")
```

This identifies the measured data point having the largest current.

---

## 26.5 Photodiode Data

A photodiode converts incident light into electrical current.

For a simple experiment, illumination can be varied and the corresponding photocurrent measured.

A useful relationship is:

$$
I_{\text{photo}}=f(L)
$$

where:

- $L$ = illumination
- $I_{\text{photo}}$ = photocurrent

### Example Dataset

```python
illumination = np.array([0, 100, 200, 300, 400, 500])

I_photo = np.array([0.8, 8.5, 16.2, 24.1, 32.0, 39.5])
```

Here illumination is expressed in lux and current in microamperes.

---

## 26.6 Plotting Photodiode Response

```python
plt.plot(illumination, I_photo, marker="o")
plt.xlabel("Illumination (lux)")
plt.ylabel("Photocurrent (µA)")
plt.title("Photodiode Response to Illumination")
plt.grid()
plt.show()
```

```{figure} ../images/ch26/ch26_photodiode_response.png
:name: ch26-photodiode-response
:align: center

Photodiode photocurrent as a function of illumination.
```

### Interpretation

The photocurrent increases approximately with illumination.

For this teaching dataset:

- zero illumination still gives a small current
- increasing illumination produces increasing photocurrent
- the relationship is approximately linear over the measured range

The small current at zero illumination can represent dark current.

---

## 26.7 Estimating Photodiode Sensitivity

A simple sensitivity measure is:

$$
S=\frac{\Delta I_{\text{photo}}}{\Delta L}
$$

Using the first and last points:

$$
S=
\frac{39.5-0.8}{500-0}
$$

Therefore,

$$
S\approx0.0774\ \mu\text{A/lux}
$$

This gives the approximate change in photocurrent per unit change in illumination over this range.

### Python Calculation

```python
sensitivity = (I_photo[-1] - I_photo[0]) / (
    illumination[-1] - illumination[0]
)

print("Sensitivity:", sensitivity, "µA/lux")
```

---

## 26.8 Solar Cell Data

A solar cell converts light energy into electrical energy.

One important measurement is the current–voltage relationship.

The solar-cell I–V curve can be written as:

$$
I=f(V)
$$

Two important quantities are:

- short-circuit current, $I_{SC}$
- open-circuit voltage, $V_{OC}$

### Example Dataset

```python
V = np.array([0.00, 0.05, 0.10, 0.15, 0.20,
              0.25, 0.30, 0.35, 0.40, 0.45, 0.50])

I_mA = np.array([45, 44.5, 43.5, 42.0, 40.0,
                 37.0, 33.0, 28.0, 20.0, 10.0, 0.0])
```

---

## 26.9 Plotting the Solar Cell I–V Curve

```python
plt.plot(V, I_mA, marker="o")
plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Solar Cell I–V Characteristic")
plt.grid()
plt.show()
```

```{figure} ../images/ch26/ch26_solar_iv.png
:name: ch26-solar-iv
:align: center

Solar cell I–V characteristic.
```

### Identifying $I_{SC}$

At approximately:

$$
V=0
$$

the current is the short-circuit current.

From the dataset:

$$
I_{SC}\approx45\text{ mA}
$$

In Python:

```python
isc = I_mA[0]

print("Short-circuit current:", isc, "mA")
```

### Identifying $V_{OC}$

The open-circuit voltage occurs when:

$$
I=0
$$

From the dataset:

$$
V_{OC}\approx0.50\text{ V}
$$

In Python:

```python
index = np.argmin(np.abs(I_mA))

voc = V[index]

print("Open-circuit voltage:", voc, "V")
```

---

## 26.10 Calculating Solar Cell Power

Electrical power is:

$$
P=VI
$$

If voltage is in volts and current is in milliamperes, the resulting power is in milliwatts.

```python
P_mW = V * I_mA
```

Let's plot the power.

```python
plt.plot(V, P_mW, marker="o")
plt.xlabel("Voltage (V)")
plt.ylabel("Power (mW)")
plt.title("Solar Cell Power–Voltage Characteristic")
plt.grid()
plt.show()
```

```{figure} ../images/ch26/ch26_solar_pv.png
:name: ch26-solar-pv
:align: center

Solar cell power–voltage characteristic.
```

---

## 26.11 Maximum Power Point

The maximum power point is the measured point where:

$$
P=VI
$$

is maximum.

```python
index = np.argmax(P_mW)

V_mp = V[index]
I_mp = I_mA[index]
P_max = P_mW[index]

print("Voltage at maximum power:", V_mp, "V")
print("Current at maximum power:", I_mp, "mA")
print("Maximum power:", P_max, "mW")
```

For this dataset:

$$
V_{MP}\approx0.25\text{ V}
$$

$$
I_{MP}\approx37\text{ mA}
$$

and

$$
P_{\max}\approx9.25\text{ mW}
$$

This is an important example of how a scientific dataset can be used to obtain a physically meaningful device parameter.

---

## 26.12 Comparing the Three Devices

| Device | Input/Control Variable | Measured Response | Typical Analysis |
|---|---|---|---|
| LED | Forward voltage | Forward current | I–V curve |
| Photodiode | Illumination | Photocurrent | Response/sensitivity |
| Solar cell | Voltage | Current and power | I–V and P–V curves |

The Python workflow is similar for all three:

$$
\text{Data}
\rightarrow
\text{Plot}
\rightarrow
\text{Calculate}
\rightarrow
\text{Interpret}
$$

---

## 26.13 Experimental Data vs Synthetic Data

The datasets in this chapter are synthetic teaching datasets.

Real laboratory data may contain:

- measurement noise
- instrument resolution limits
- temperature effects
- contact resistance
- calibration errors
- fluctuations in illumination
- device-to-device variation

Therefore, real data will usually not produce perfectly smooth curves.

The purpose of the present examples is to learn the analysis workflow before working with larger experimental datasets.

---

## 26.14 A Complete Mini Analysis

The following pattern can be reused for semiconductor device experiments.

```python
import numpy as np
import matplotlib.pyplot as plt

# Load or enter experimental data
V = np.array([0.00, 0.10, 0.20, 0.30, 0.40, 0.50])
I = np.array([45, 43.5, 40.0, 33.0, 20.0, 0.0])

# Calculate power
P = V * I

# Find maximum power
index = np.argmax(P)

print("Maximum power:", P[index], "mW")
print("Voltage:", V[index], "V")
print("Current:", I[index], "mA")

# Plot
plt.plot(V, I, marker="o")
plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Device I–V Characteristic")
plt.grid()
plt.show()
```

The important point is not only obtaining the numerical answer.

A scientific analysis should also explain **what the answer means physically**.

---

## 26.15 Common Mistakes

### Mistake 1: Mixing units

Do not treat mA as A without conversion.

$$
1\text{ mA}=10^{-3}\text{ A}
$$

### Mistake 2: Ignoring axis labels

Always include:

- variable name
- unit
- meaningful title

### Mistake 3: Confusing $I_{SC}$ and $V_{OC}$

Remember:

$$
I_{SC}: V=0
$$

and

$$
V_{OC}: I=0
$$

### Mistake 4: Reporting only a number

A scientific result should include its meaning and unit.

For example:

> The maximum measured solar-cell power is approximately 9.25 mW at 0.25 V.

---

## 26.16 Key Points

- LED data can be analyzed using an I–V characteristic.
- Photodiode current can be studied as a function of illumination.
- Solar-cell data can be analyzed using I–V and P–V curves.
- $I_{SC}$ occurs at approximately zero voltage.
- $V_{OC}$ occurs at approximately zero current.
- Solar-cell power is calculated using $P=VI$.
- `np.argmax()` is useful for finding a maximum measured value.
- Scientific interpretation is as important as calculation.

---

## 26.17 Quick Practice

1. What does an LED I–V curve represent?
2. What is the difference between dark current and photocurrent?
3. Write the formula for photodiode sensitivity.
4. What is short-circuit current?
5. What is open-circuit voltage?
6. Write the equation for electrical power.
7. Why is a P–V curve useful for a solar cell?
8. What does `np.argmax()` do?

---

## 26.18 Hands-on Activity

Use an experimental or synthetic solar-cell dataset.

1. Enter the voltage and current values into NumPy arrays.
2. Plot the I–V curve.
3. Calculate power using $P=VI$.
4. Plot the P–V curve.
5. Find the maximum power point.
6. Report $I_{SC}$ and $V_{OC}$.
7. Write three sentences interpreting the device behaviour.

---

## 26.19 Chapter Summary

In this chapter, we applied Python to three important semiconductor devices: LED, photodiode, and solar cell.

The main workflow was:

$$
\boxed{
\text{Device Data}
\rightarrow
\text{Python}
\rightarrow
\text{Visualization}
\rightarrow
\text{Calculation}
\rightarrow
\text{Physical Interpretation}
}
$$

This workflow will be used throughout the remaining chapters of Unit II.
