# Chapter 28 — Mobility-Related Data Analysis

## 28.1 Introduction

Carrier mobility describes how easily charge carriers move through a semiconductor when an electric field is applied.

It is an important parameter in semiconductor device analysis because carrier motion affects electrical conductivity and device performance.

In this chapter, we will use experimental-style data to understand:

- carrier mobility
- Hall-effect data
- Hall coefficient
- carrier concentration
- conductivity–mobility relationship
- temperature dependence of mobility

The main workflow is:

$$
\text{Measured Data}
\rightarrow
\text{Python}
\rightarrow
\text{Fit}
\rightarrow
\text{Calculate}
\rightarrow
\text{Material Parameter}
$$

The datasets in the examples are **synthetic teaching datasets**.

---

## 28.2 What Is Carrier Mobility?

Carrier mobility measures how quickly a charge carrier responds to an applied electric field.

It is represented by:

$$
\mu
$$

A simple relationship is:

$$
v_d=\mu E
$$

where:

- $v_d$ = drift velocity
- $\mu$ = mobility
- $E$ = electric field

Therefore:

$$
\mu=\frac{v_d}{E}
$$

The SI unit of mobility is:

$$
m^2/(V\,s)
$$

---

## 28.3 Mobility and Conductivity

Electrical conductivity is related to carrier concentration and mobility.

For a simple single-carrier model:

$$
\sigma=qn\mu
$$

where:

- $\sigma$ = conductivity
- $q$ = elementary charge
- $n$ = carrier concentration
- $\mu$ = carrier mobility

This equation is very useful because it connects measurable electrical properties with semiconductor material parameters.

---

## 28.4 Example

Suppose:

$$
q=1.6\times10^{-19}\text{ C}
$$

$$
n=5\times10^{20}\text{ m}^{-3}
$$

and

$$
\mu=0.08\text{ m}^2/(V\,s)
$$

Then:

$$
\sigma=qn\mu
$$

Using Python:

```python
q = 1.602176634e-19
n = 5e20
mu = 0.08

sigma = q * n * mu

print("Conductivity:", sigma, "S/m")
```

The calculation shows how increasing either carrier concentration or mobility can increase conductivity.

---

## 28.5 Conductivity and Carrier Concentration

For constant mobility:

$$
\sigma=qn\mu
$$

Therefore:

$$
\sigma\propto n
$$

This means conductivity increases linearly with carrier concentration when mobility is approximately constant.

```python
import numpy as np
import matplotlib.pyplot as plt

q = 1.602176634e-19

n = np.array([1e20, 2e20, 5e20, 1e21, 2e21])

mu = 0.08

sigma = q * n * mu

plt.plot(n, sigma, marker="o")
plt.xlabel("Carrier Concentration n (m⁻³)")
plt.ylabel("Conductivity σ (S/m)")
plt.title("Conductivity vs Carrier Concentration")
plt.grid()
plt.show()
```

```{figure} ../images/ch28/ch28_conductivity_carrier_concentration.png
:name: ch28-conductivity-carrier-concentration
:align: center

Conductivity increases with carrier concentration when mobility is held constant.
```

---

## 28.6 Conductivity and Mobility

Similarly, if carrier concentration is approximately constant:

$$
\sigma\propto\mu
$$

Consider:

```python
mu = np.array([0.02, 0.04, 0.06, 0.08, 0.10])

n = 5e20

sigma = q * n * mu
```

Plotting the relationship:

```python
plt.plot(mu, sigma, marker="o")
plt.xlabel("Mobility μ (m²/Vs)")
plt.ylabel("Conductivity σ (S/m)")
plt.title("Conductivity vs Carrier Mobility")
plt.grid()
plt.show()
```

```{figure} ../images/ch28/ch28_conductivity_mobility.png
:name: ch28-conductivity-mobility
:align: center

Conductivity increases with mobility when carrier concentration is held constant.
```

The important relationship is:

$$
\boxed{\sigma=qn\mu}
$$

---

## 28.7 What Is the Hall Effect?

The Hall effect provides a useful experimental method for studying charge carriers in a semiconductor.

A sample carrying current is placed in a magnetic field.

A voltage develops across the sample in a direction perpendicular to both the current and magnetic field.

This voltage is called the **Hall voltage**:

$$
V_H
$$

A simplified relationship is:

$$
V_H=R_H\frac{IB}{t}
$$

where:

- $R_H$ = Hall coefficient
- $I$ = current
- $B$ = magnetic field
- $t$ = sample thickness

Rearranging:

$$
R_H=\frac{V_Ht}{IB}
$$

---

## 28.8 Why Is Hall Data Useful?

Hall measurements can provide information about:

- carrier concentration
- carrier type
- Hall coefficient
- mobility

For a simple single-carrier model:

$$
R_H=\frac{1}{qn}
$$

Therefore:

$$
n=\frac{1}{q|R_H|}
$$

The sign of the Hall coefficient can also provide information about the dominant carrier type.

For the simplified discussion here, we focus mainly on the magnitude.

---

## 28.9 Example Hall Dataset

Suppose a semiconductor sample is measured at a fixed current.

```python
B = np.array([0.0, 0.10, 0.20, 0.30, 0.40, 0.50])

V_H = np.array([0.000, 0.012, 0.024,
                0.0365, 0.0475, 0.060])
```

Here:

- `B` = magnetic field in tesla
- `V_H` = Hall voltage in volts

We can first visualize the relationship.

```python
plt.plot(B, V_H, marker="o")
plt.xlabel("Magnetic Field B (T)")
plt.ylabel("Hall Voltage V_H (V)")
plt.title("Hall Voltage vs Magnetic Field")
plt.grid()
plt.show()
```

---

## 28.10 Linear Fitting of Hall Data

The Hall equation can be rearranged as:

$$
V_H=
\left(\frac{R_HI}{t}\right)B
$$

This has the form:

$$
y=ax+b
$$

Therefore, a plot of $V_H$ against $B$ can be analyzed using linear fitting.

```python
slope, intercept = np.polyfit(B, V_H, 1)

print("Slope:", slope)
print("Intercept:", intercept)
```

Plot the measured data and fitted line:

```python
fit = slope * B + intercept

plt.plot(B, V_H, marker="o", label="Measured data")
plt.plot(B, fit, label="Linear fit")

plt.xlabel("Magnetic Field B (T)")
plt.ylabel("Hall Voltage V_H (V)")
plt.title("Hall Voltage vs Magnetic Field")
plt.grid()
plt.legend()
plt.show()
```

```{figure} ../images/ch28/ch28_hall_voltage_fit.png
:name: ch28-hall-voltage-fit
:align: center

Hall voltage as a function of magnetic field with a linear fit.
```

### Interpretation

The approximately straight-line relationship means that Hall voltage increases approximately linearly with magnetic field over this measured range.

The slope contains physical information about the semiconductor sample.

---

## 28.11 Calculating the Hall Coefficient

From:

$$
V_H=R_H\frac{IB}{t}
$$

the slope of the $V_H$ versus $B$ graph is:

$$
\text{slope}=\frac{R_HI}{t}
$$

Therefore:

$$
R_H=\frac{\text{slope}\,t}{I}
$$

Suppose:

```python
I = 0.010
t = 0.001
```

Then:

```python
R_H = slope * t / I

print("Hall coefficient:", R_H, "m³/C")
```

---

## 28.12 Estimating Carrier Concentration

Using:

$$
n=\frac{1}{q|R_H|}
$$

we can calculate the carrier concentration.

```python
q = 1.602176634e-19

n = 1 / (q * abs(R_H))

print("Carrier concentration:", n, "m^-3")
```

This illustrates an important scientific workflow:

$$
V_H
\rightarrow
\text{slope}
\rightarrow
R_H
\rightarrow
n
$$

A measured voltage has been converted into a material parameter.

---

## 28.13 Calculating Mobility

Once conductivity and carrier concentration are known, mobility can be obtained from:

$$
\sigma=qn\mu
$$

Therefore:

$$
\mu=\frac{\sigma}{qn}
$$

Suppose the measured resistivity is:

```python
rho = 0.020
```

Then:

```python
sigma = 1 / rho

mu = sigma / (q * n)

print("Conductivity:", sigma, "S/m")
print("Mobility:", mu, "m²/Vs")
```

The complete chain is:

$$
R
\rightarrow
\rho
\rightarrow
\sigma
$$

and

$$
V_H
\rightarrow
R_H
\rightarrow
n
$$

then:

$$
\sigma,n
\rightarrow
\mu
$$

---

## 28.14 Mobility and Temperature

Carrier mobility can also depend on temperature.

For a teaching dataset:

```python
T = np.array([280, 290, 300, 310, 320, 330])

mu = np.array([
    0.112,
    0.103,
    0.095,
    0.088,
    0.082,
    0.076
])
```

Plot the data:

```python
plt.plot(T, mu, marker="o")
plt.xlabel("Temperature (K)")
plt.ylabel("Mobility μ (m²/Vs)")
plt.title("Carrier Mobility vs Temperature")
plt.grid()
plt.show()
```

```{figure} ../images/ch28/ch28_mobility_temperature.png
:name: ch28-mobility-temperature
:align: center

Illustrative carrier mobility as a function of temperature.
```

### Interpretation

For this teaching dataset, mobility decreases as temperature increases.

The physical reason can involve increased lattice vibrations and increased carrier scattering.

The exact temperature dependence depends on the material and the dominant scattering mechanisms.

---

## 28.15 Why Mobility Matters in Semiconductor Devices

Mobility affects how easily carriers move through a semiconductor.

Higher mobility can contribute to:

- higher conductivity
- faster carrier transport
- lower resistive losses
- changes in transistor and sensor behaviour

However, mobility should not be interpreted independently of carrier concentration.

Remember:

$$
\sigma=qn\mu
$$

A material can have high mobility but relatively low conductivity if its carrier concentration is low.

---

## 28.16 Complete Mini Analysis

The following example combines the main calculations.

```python
import numpy as np

# Hall data
B = np.array([0.0, 0.10, 0.20, 0.30, 0.40, 0.50])

V_H = np.array([
    0.000, 0.012, 0.024,
    0.0365, 0.0475, 0.060
])

# Linear fit
slope, intercept = np.polyfit(B, V_H, 1)

# Experimental conditions
I = 0.010
t = 0.001

# Hall coefficient
R_H = slope * t / I

# Carrier concentration
q = 1.602176634e-19
n = 1 / (q * abs(R_H))

# Resistivity and conductivity
rho = 0.020
sigma = 1 / rho

# Mobility
mu = sigma / (q * n)

print("Slope:", slope)
print("Hall coefficient:", R_H, "m³/C")
print("Carrier concentration:", n, "m^-3")
print("Conductivity:", sigma, "S/m")
print("Mobility:", mu, "m²/Vs")
```

This is a good example of how several simple calculations can be combined into a scientific analysis workflow.

---

## 28.17 Important Units

Keep track of units throughout the calculation.

| Quantity | Symbol | SI Unit |
|---|---|---|
| Magnetic field | $B$ | T |
| Hall voltage | $V_H$ | V |
| Current | $I$ | A |
| Thickness | $t$ | m |
| Hall coefficient | $R_H$ | m³/C |
| Carrier concentration | $n$ | m⁻³ |
| Conductivity | $\sigma$ | S/m |
| Mobility | $\mu$ | m²/(V s) |

Unit checking is one of the simplest ways to identify calculation errors.

---

## 28.18 Common Mistakes

### Mistake 1: Confusing mobility and conductivity

They are related, but they are not the same quantity.

$$
\sigma=qn\mu
$$

### Mistake 2: Using current in mA without conversion

The Hall equation requires current in the appropriate SI units when calculating SI quantities.

$$
1\text{ mA}=10^{-3}\text{ A}
$$

### Mistake 3: Ignoring sample thickness

The Hall coefficient calculation requires the sample thickness:

$$
R_H=\frac{\text{slope}\,t}{I}
$$

### Mistake 4: Forgetting the absolute value

When calculating carrier concentration using the simplified expression:

$$
n=\frac{1}{q|R_H|}
$$

the magnitude is used.

The sign of $R_H$ contains additional information about the dominant carrier type.

### Mistake 5: Treating synthetic data as real experimental evidence

The datasets in this chapter are for learning the analysis method. A real experiment requires measurement conditions, calibration information, uncertainty, and appropriate physical interpretation.

---

## 28.19 Key Points

- Mobility describes carrier response to an electric field.
- Drift velocity and mobility are related by:

$$
v_d=\mu E
$$

- Conductivity is related to carrier concentration and mobility:

$$
\sigma=qn\mu
$$

- Hall measurements can be used to estimate carrier properties.
- Hall voltage can be analyzed using linear fitting.
- The slope of $V_H$ versus $B$ is related to the Hall coefficient.
- Carrier concentration can be estimated from the Hall coefficient.
- Mobility can be calculated from conductivity and carrier concentration.
- Mobility can depend on temperature.
- Units and experimental conditions are essential for meaningful results.

---

## 28.20 Quick Practice

1. What does carrier mobility represent?
2. Write the relationship between drift velocity and mobility.
3. Write the conductivity equation involving $n$ and $\mu$.
4. What is the Hall effect?
5. Why is $V_H$ plotted against $B$?
6. What does the slope of a $V_H$ versus $B$ graph represent?
7. How can carrier concentration be estimated from Hall data?
8. How is mobility calculated when conductivity and carrier concentration are known?
9. Why can mobility change with temperature?
10. What is the SI unit of mobility?

---

## 28.21 Hands-on Activity

Use the Hall-effect dataset provided in this chapter.

1. Enter $B$ and $V_H$ into NumPy arrays.
2. Plot $V_H$ against $B$.
3. Perform a linear fit.
4. Extract the slope.
5. Calculate the Hall coefficient.
6. Estimate carrier concentration.
7. Use a measured resistivity value to calculate conductivity.
8. Calculate mobility.
9. Report all values with units.
10. Write a short scientific interpretation.

---

## 28.22 Chapter Summary

Mobility-related analysis shows how electrical measurements can be converted into semiconductor material parameters.

The main workflow is:

$$
\boxed{
V_H
\rightarrow
\text{Linear Fit}
\rightarrow
R_H
\rightarrow
n
\rightarrow
\sigma
\rightarrow
\mu
}
$$

The central relationship is:

$$
\boxed{\sigma=qn\mu}
$$

This connects three important semiconductor properties:

**carrier concentration, mobility, and conductivity.**

The next chapter will focus on **error and uncertainty analysis**, where we will learn how to describe the reliability of experimental measurements and calculated results.
