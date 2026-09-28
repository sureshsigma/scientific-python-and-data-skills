# Chapter 27 — Semiconductor Material Parameters

## 27.1 Introduction

The electrical behaviour of a semiconductor can be described using several important material parameters.

In this chapter, we will work with:

- resistance
- resistivity
- conductivity
- temperature dependence
- band gap estimation

The main idea is:

$$
\text{Experimental Data}
\rightarrow
\text{Python}
\rightarrow
\text{Calculation}
\rightarrow
\text{Plot}
\rightarrow
\text{Material Interpretation}
$$

---

## 27.2 Resistance and Resistivity

Resistance describes how strongly a particular sample opposes electric current.

The resistance of a uniform sample is related to resistivity by:

$$
R=\rho\frac{L}{A}
$$

where:

- $R$ = resistance in $\Omega$
- $\rho$ = resistivity in $\Omega\,m$
- $L$ = length of the sample in m
- $A$ = cross-sectional area in $m^2$

Rearranging:

$$
\rho=R\frac{A}{L}
$$

This equation is important because resistance depends on the **size and shape of the sample**, whereas resistivity is a material property.

---

## 27.3 Example Experimental Data

Suppose a semiconductor sample has:

$$
L=0.005\text{ m}
$$

and

$$
A=1\times10^{-6}\text{ m}^2
$$

The measured resistance at different temperatures is:

```python
import numpy as np

T = np.array([280, 290, 300, 310, 320, 330])

R = np.array([125, 104, 86, 71, 59, 49])
```

Here:

- `T` = temperature in K
- `R` = measured resistance in ohms

---

## 27.4 Calculating Resistivity

Use:

$$
\rho=R\frac{A}{L}
$$

in Python:

```python
L = 0.005
A = 1e-6

rho = R * A / L

print(rho)
```

For the first measurement:

$$
\rho
=
125\frac{1\times10^{-6}}{0.005}
$$

Therefore:

$$
\rho=0.025\ \Omega m
$$

The same calculation can be performed for every temperature.

---

## 27.5 Plotting Resistivity

```python
import matplotlib.pyplot as plt

plt.plot(T, rho, marker="o")
plt.xlabel("Temperature (K)")
plt.ylabel("Resistivity (Ω m)")
plt.title("Semiconductor Resistivity vs Temperature")
plt.grid()
plt.show()
```

```{figure} ../images/ch27/ch27_resistivity_temperature.png
:name: ch27-resistivity-temperature
:align: center

Semiconductor resistivity as a function of temperature.
```

### Interpretation

For this teaching dataset, resistivity decreases as temperature increases.

This is a common qualitative behaviour of semiconductor materials.

The important point is that the graph allows us to see the temperature dependence directly from experimental data.

---

## 27.6 Conductivity

Conductivity measures how easily electric current can flow through a material.

Conductivity is the reciprocal of resistivity:

$$
\sigma=\frac{1}{\rho}
$$

where:

- $\sigma$ = conductivity
- $\rho$ = resistivity

The SI unit of conductivity is:

$$
S/m
$$

### Python

```python
sigma = 1 / rho

print(sigma)
```

For example, if:

$$
\rho=0.025\ \Omega m
$$

then:

$$
\sigma=\frac{1}{0.025}
=40\ S/m
$$

---

## 27.7 Plotting Conductivity

```python
plt.plot(T, sigma, marker="o")
plt.xlabel("Temperature (K)")
plt.ylabel("Conductivity (S/m)")
plt.title("Semiconductor Conductivity vs Temperature")
plt.grid()
plt.show()
```

```{figure} ../images/ch27/ch27_conductivity_temperature.png
:name: ch27-conductivity-temperature
:align: center

Semiconductor conductivity as a function of temperature.
```

### Interpretation

Since:

$$
\sigma=\frac{1}{\rho}
$$

a decrease in resistivity produces an increase in conductivity.

For this dataset:

- temperature increases
- resistivity decreases
- conductivity increases

---

## 27.8 Resistivity and Conductivity Are Related

The two quantities contain essentially reciprocal information:

$$
\boxed{\sigma=\frac{1}{\rho}}
$$

and

$$
\boxed{\rho=\frac{1}{\sigma}}
$$

Therefore, when analyzing experimental data, it is important to keep the relationship between the two quantities in mind.

---

## 27.9 Visualizing the Relationship

```python
rho_demo = np.array([1, 2, 5, 10, 20])

sigma_demo = 1 / rho_demo

plt.plot(rho_demo, sigma_demo, marker="o")
plt.xlabel("Resistivity (Ω m)")
plt.ylabel("Conductivity (S/m)")
plt.title("Relationship Between Resistivity and Conductivity")
plt.grid()
plt.show()
```

```{figure} ../images/ch27/ch27_resistivity_conductivity.png
:name: ch27-resistivity-conductivity
:align: center

Reciprocal relationship between resistivity and conductivity.
```

The graph shows that higher resistivity corresponds to lower conductivity.

---

## 27.10 Temperature Dependence of Semiconductor Conductivity

For an intrinsic semiconductor, conductivity has a strong temperature dependence.

A simplified relationship can be written as:

$$
\sigma \propto
e^{-E_g/(2k_BT)}
$$

where:

- $E_g$ = band gap energy
- $k_B$ = Boltzmann constant
- $T$ = absolute temperature

Taking the natural logarithm gives a useful linear form:

$$
\ln(\sigma)
=
\ln(\sigma_0)
-
\frac{E_g}{2k_B}\frac{1}{T}
$$

This is useful because it converts the temperature relationship into a form that can be analyzed using linear fitting.

---

## 27.11 Why Plot $\ln(\sigma)$ Against $1/T$?

Compare the equation:

$$
\ln(\sigma)
=
\ln(\sigma_0)
-
\frac{E_g}{2k_B}\frac{1}{T}
$$

with a straight-line equation:

$$
y=a+bx
$$

We can identify:

$$
y=\ln(\sigma)
$$

and

$$
x=\frac{1}{T}
$$

Therefore:

$$
\text{slope}
=
-\frac{E_g}{2k_B}
$$

So:

$$
E_g=-2k_B(\text{slope})
$$

This gives a simple method for estimating the band gap from conductivity data.

---

## 27.12 Preparing the Data

```python
x = 1 / T
y = np.log(sigma)
```

Now:

- `x` contains $1/T$
- `y` contains $\ln(\sigma)$

We can perform a linear fit.

```python
slope, intercept = np.polyfit(x, y, 1)

print("Slope:", slope)
print("Intercept:", intercept)
```

---

## 27.13 Estimating the Band Gap

Use:

```python
k_B = 8.617333262e-5   # eV/K

Eg = -2 * k_B * slope

print("Estimated band gap:", Eg, "eV")
```

The calculated band gap is obtained from the slope of the fitted line.

The important idea is not memorizing the Python command.

The important scientific chain is:

$$
\sigma(T)
\rightarrow
\ln(\sigma)
\rightarrow
\frac{1}{T}
\rightarrow
\text{linear fit}
\rightarrow
E_g
$$

---

## 27.14 Plotting the Band Gap Analysis

```python
plt.plot(x, y, marker="o", label="Data")
plt.plot(x, slope*x + intercept, label="Linear fit")

plt.xlabel("1 / T (K⁻¹)")
plt.ylabel("ln(σ)")
plt.title("Band Gap Estimation from Conductivity Data")
plt.grid()
plt.legend()
plt.show()
```

```{figure} ../images/ch27/ch27_band_gap_estimation.png
:name: ch27-band-gap-estimation
:align: center

Linearized conductivity data used for band gap estimation.
```

### Interpretation

If the transformed data approximately follow a straight line, the linear model provides a way to estimate the band gap.

However, real experimental data may not produce a perfectly straight line.

Possible reasons include:

- measurement noise
- temperature measurement error
- contact resistance
- non-intrinsic conduction
- limited temperature range
- material impurities
- experimental setup effects

Therefore, the estimated band gap should be interpreted together with the quality of the experimental data and the fitted model.

---

## 27.15 Why Units Matter

Scientific calculations require consistent units.

For example:

$$
L=5\text{ mm}
$$

must be converted to:

$$
L=0.005\text{ m}
$$

before using the SI form of:

$$
\rho=R\frac{A}{L}
$$

Similarly:

$$
1\text{ mm}^2=10^{-6}\text{ m}^2
$$

Incorrect unit conversion can produce a numerically correct-looking but scientifically incorrect answer.

---

## 27.16 A Complete Python Example

```python
import numpy as np
import matplotlib.pyplot as plt

# Experimental data
T = np.array([280, 290, 300, 310, 320, 330])
R = np.array([125, 104, 86, 71, 59, 49])

# Sample dimensions
L = 0.005
A = 1e-6

# Resistivity
rho = R * A / L

# Conductivity
sigma = 1 / rho

# Band-gap transformation
x = 1 / T
y = np.log(sigma)

# Linear fit
slope, intercept = np.polyfit(x, y, 1)

# Band gap
k_B = 8.617333262e-5
Eg = -2 * k_B * slope

print("Resistivity:", rho)
print("Conductivity:", sigma)
print("Slope:", slope)
print("Estimated band gap:", Eg, "eV")

# Plot
plt.plot(x, y, marker="o", label="Data")
plt.plot(x, slope*x + intercept, label="Linear fit")
plt.xlabel("1 / T (K⁻¹)")
plt.ylabel("ln(σ)")
plt.title("Band Gap Estimation")
plt.grid()
plt.legend()
plt.show()
```

---

## 27.17 Scientific Interpretation

Suppose the analysis gives an estimated band gap.

A scientific report should not simply state:

> Band gap = ___ eV.

Instead, write something like:

> The temperature-dependent conductivity data were transformed by plotting $\ln(\sigma)$ against $1/T$. A linear fit was then used to estimate the semiconductor band gap from the fitted slope.

This connects:

**data → method → calculation → physical meaning**

---

## 27.18 Important Distinction

### Resistance

Depends on the physical dimensions of the sample.

$$
R=\rho\frac{L}{A}
$$

### Resistivity

A material property.

$$
\rho=R\frac{A}{L}
$$

### Conductivity

The reciprocal of resistivity.

$$
\sigma=\frac{1}{\rho}
$$

Keeping these three concepts separate is important when analyzing semiconductor experiments.

---

## 27.19 Common Mistakes

### Mistake 1: Using resistance as resistivity

Resistance and resistivity are not the same quantity.

### Mistake 2: Forgetting sample dimensions

You need $L$ and $A$ to calculate resistivity from resistance.

### Mistake 3: Mixing units

Convert mm to m and mm² to m² before using SI equations.

### Mistake 4: Using Celsius in the band-gap equation

Temperature in:

$$
\frac{1}{T}
$$

must be in **kelvin**, not °C.

Conversion:

$$
T(K)=T(^\circ C)+273.15
$$

### Mistake 5: Interpreting every fitted line as exact physics

A fitted relationship is an approximation to measured data. Its physical validity depends on the experimental conditions and assumptions of the model.

---

## 27.20 Key Points

- Resistance depends on sample dimensions.
- Resistivity is calculated using:

$$
\rho=R\frac{A}{L}
$$

- Conductivity is:

$$
\sigma=\frac{1}{\rho}
$$

- Semiconductor conductivity is temperature dependent.
- Band-gap estimation can use a plot of $\ln(\sigma)$ against $1/T$.
- The slope of the linear fit is related to $E_g$.
- Temperature must be expressed in kelvin.
- Units are an essential part of scientific data analysis.
- A fitted parameter should always be interpreted physically.

---

## 27.21 Quick Practice

1. Write the relationship between resistance and resistivity.
2. What is the SI unit of resistivity?
3. What is the SI unit of conductivity?
4. If resistivity decreases, what happens to conductivity?
5. Why must temperature be converted to kelvin?
6. Why is $1/T$ used for band-gap estimation?
7. What quantity is plotted on the y-axis in the linearized band-gap method?
8. Why can experimental data deviate from a straight line?

---

## 27.22 Hands-on Activity

Use a semiconductor resistance–temperature dataset.

1. Enter temperature and resistance data into NumPy.
2. Convert temperature to kelvin if necessary.
3. Calculate resistivity.
4. Calculate conductivity.
5. Plot resistivity against temperature.
6. Plot conductivity against temperature.
7. Calculate $1/T$ and $\ln(\sigma)$.
8. Perform a linear fit.
9. Estimate the band gap.
10. Write a short scientific interpretation of the result.

---

## 27.23 Chapter Summary

In this chapter, we used Python to calculate and analyze important semiconductor material parameters.

The workflow was:

$$
\boxed{
R
\rightarrow
\rho
\rightarrow
\sigma
\rightarrow
\ln(\sigma)
\text{ vs }
1/T
\rightarrow
\text{Linear Fit}
\rightarrow
E_g
}
$$

This provides a practical example of how experimental electrical measurements can be converted into meaningful semiconductor material information.
