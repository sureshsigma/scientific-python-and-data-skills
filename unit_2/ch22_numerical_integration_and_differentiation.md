# Chapter 22 — Numerical Integration and Differentiation

## 22.1 Introduction

Scientific experiments often produce a set of numerical measurements rather than a mathematical equation.

For example, an instrument may record current at different times:

| Time (s) | Current (A) |
|---:|---:|
| 0 | 0.0 |
| 1 | 0.5 |
| 2 | 1.0 |
| 3 | 1.5 |
| 4 | 1.2 |
| 5 | 0.8 |

From these measurements, we may want to calculate:

- area under a curve
- total charge
- accumulated quantities
- rate of change
- other physical quantities derived from measured data

Two important numerical operations are **numerical integration** and **numerical differentiation**.

The basic workflow is:

$$
\boxed{
\text{Experimental Data}
\rightarrow
\text{Numerical Calculation}
\rightarrow
\text{Physical Quantity}
\rightarrow
\text{Interpretation}
}
$$

---

## 22.2 What Is Numerical Integration?

Integration can be used to calculate the area under a curve.

Mathematically:

$$
A=\int_a^b f(x)\,dx
$$

Experimental data usually consists of discrete measurements rather than a continuous mathematical function.

For example:

```text
x       y
0       0.0
1       0.5
2       1.0
3       1.5
4       1.2
5       0.8
```

In this situation, we can use **numerical integration**.

---

## 22.3 Why Is Integration Useful in Electronics?

Integration appears in many scientific applications.

For example, electrical charge is related to current by:

$$
Q=\int I(t)\,dt
$$

where:

- $Q$ = charge
- $I(t)$ = current
- $t$ = time

Therefore, if an experiment records current as a function of time, numerical integration can estimate the total charge.

---

## 22.4 Experimental Current–Time Data

Consider:

```python
import numpy as np

t = np.array([0, 1, 2, 3, 4, 5])
I = np.array([0.0, 0.5, 1.0, 1.5, 1.2, 0.8])
```

We can plot the measurements:

```python
import matplotlib.pyplot as plt

plt.plot(t, I, marker="o")
plt.fill_between(t, I, 0, alpha=0.25)
plt.xlabel("Time (s)")
plt.ylabel("Current (A)")
plt.title("Current–Time Data")

plt.show()
```

The measured points represent the experimental observations.

---

## 22.5 Area Under the Curve

The area under the current–time curve represents:

$$
Q=\int I(t)\,dt
$$

For discrete experimental measurements, we approximate this area numerically.

```{figure} ../images/ch22/ch22_current_time_area.png
:alt: Current-time data with area under the curve
:name: ch22-current-time-area

Current–time measurements and the area used for numerical integration.
```

The shaded area represents the quantity being approximated by numerical integration.

---

## 22.6 The Trapezoidal Rule

One simple numerical integration method is the **trapezoidal rule**.

For two points:

$$
(x_1,y_1)
$$

and

$$
(x_2,y_2)
$$

the approximate area is:

$$
A
\approx
\frac{y_1+y_2}{2}(x_2-x_1)
$$

The region between the two points is approximated by a trapezoid.

For many measurements, we add the areas of all adjacent trapezoids.

Therefore:

$$
\boxed{
\text{Total Area}
\approx
\sum
\text{Trapezoid Areas}
}
$$

---

## 22.7 Manual Calculation for One Interval

Consider:

$$
t_1=1\text{ s}
$$

with:

$$
I_1=0.5\text{ A}
$$

and:

$$
t_2=2\text{ s}
$$

with:

$$
I_2=1.0\text{ A}
$$

The interval is:

$$
\Delta t=2-1=1\text{ s}
$$

The trapezoidal area is:

$$
Q_1
=
\frac{0.5+1.0}{2}(1)
$$

Therefore:

$$
Q_1=0.75\text{ C}
$$

because:

$$
\text{A}\times\text{s}=\text{C}
$$

---

## 22.8 Numerical Integration with SciPy

SciPy provides numerical integration tools through:

```python
scipy.integrate
```

For tabulated experimental data, a convenient function is:

```python
trapezoid
```

Use:

```python
from scipy.integrate import trapezoid
```

Then:

```python
charge = trapezoid(I, t)

print("Charge =", charge, "C")
```

This calculates the total area under the current–time curve.

---

## 22.9 Cumulative Integration

Sometimes we do not want only the final total.

We want to know how the quantity accumulates with time.

SciPy provides:

```python
cumulative_trapezoid
```

Example:

```python
from scipy.integrate import cumulative_trapezoid

charge = cumulative_trapezoid(
    I,
    t,
    initial=0
)

print(charge)
```

The result gives cumulative charge at each time point.

We can plot it:

```python
plt.plot(t, charge, marker="o")

plt.xlabel("Time (s)")
plt.ylabel("Charge (C)")
plt.title("Cumulative Charge")

plt.show()
```

```{figure} ../images/ch22/ch22_cumulative_charge.png
:alt: Cumulative charge calculated from current-time data
:name: ch22-cumulative-charge

Cumulative charge obtained by numerically integrating the current–time measurements.
```

The curve shows how charge accumulates as time increases.

---

## 22.10 What Is Numerical Differentiation?

Integration looks at accumulation.

Differentiation looks at **rate of change**.

Mathematically:

$$
\frac{dy}{dx}
$$

represents the rate at which $y$ changes with respect to $x$.

For experimental data, we may have discrete observations rather than a mathematical function.

Numerical differentiation allows us to estimate the derivative from those measurements.

---

## 22.11 Simple Numerical Derivative

Suppose we have two measurements:

$$
(x_1,y_1)
$$

and

$$
(x_2,y_2)
$$

A simple estimate of the slope is:

$$
\frac{\Delta y}{\Delta x}
=
\frac{y_2-y_1}{x_2-x_1}
$$

This is the average rate of change between the two measurements.

---

## 22.12 Example

Suppose:

$$
x_1=1,\quad y_1=1
$$

and:

$$
x_2=2,\quad y_2=4
$$

Then:

$$
\frac{\Delta y}{\Delta x}
=
\frac{4-1}{2-1}
$$

Therefore:

$$
\frac{\Delta y}{\Delta x}=3
$$

This tells us that the average rate of change between $x=1$ and $x=2$ is 3.

---

## 22.13 Numerical Differentiation with NumPy

NumPy provides:

```python
np.gradient()
```

for estimating derivatives from numerical data.

Consider:

```python
x = np.array([0, 1, 2, 3, 4, 5])
V = np.array([0, 1, 4, 9, 16, 25])
```

Calculate the numerical derivative:

```python
dVdx = np.gradient(V, x)

print(dVdx)
```

The result gives an estimate of:

$$
\frac{dV}{dx}
$$

at the measured points.

---

## 22.14 Visualizing the Numerical Derivative

```python
plt.plot(x, V, marker="o", label="V(x)")
plt.plot(x, dVdx, marker="s", label="Numerical derivative")

plt.xlabel("Position x")
plt.ylabel("Value")
plt.title("Numerical Differentiation")
plt.legend()

plt.show()
```

```{figure} ../images/ch22/ch22_numerical_differentiation.png
:alt: Numerical differentiation of discrete data
:name: ch22-numerical-differentiation

Original discrete data and its numerical derivative estimated using NumPy.
```

The derivative describes how rapidly the original quantity changes.

---

## 22.15 Integration vs Differentiation

The two operations answer different questions.

### Integration

Integration asks:

> How much has accumulated?

Example:

$$
Q=\int I(t)\,dt
$$

### Differentiation

Differentiation asks:

> How quickly is something changing?

Example:

$$
\frac{dV}{dx}
$$

A useful summary is:

$$
\boxed{
\text{Integration}
\rightarrow
\text{Accumulation}
}
$$

$$
\boxed{
\text{Differentiation}
\rightarrow
\text{Rate of Change}
}
$$

---

## 22.16 Units Are Important

Numerical calculations should always be interpreted with units.

For current and time:

$$
Q=\int I\,dt
$$

If:

$$
I\text{ is in A}
$$

and:

$$
t\text{ is in s}
$$

then:

$$
Q\text{ is in C}
$$

because:

$$
1\text{ A}\cdot1\text{ s}=1\text{ C}
$$

Similarly, if:

$$
V\text{ is in volts}
$$

and:

$$
x\text{ is in meters}
$$

then:

$$
\frac{dV}{dx}
$$

has units:

$$
\text{V/m}
$$

Never interpret a numerical derivative or integral without checking its units.

---

## 22.17 Complete Integration Example

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import trapezoid

t = np.array([0, 1, 2, 3, 4, 5])
I = np.array([0.0, 0.5, 1.0, 1.5, 1.2, 0.8])

# Numerical integration
Q = trapezoid(I, t)

print("Total charge =", Q, "C")

# Plot
plt.plot(t, I, marker="o")

plt.xlabel("Time (s)")
plt.ylabel("Current (A)")
plt.title("Current–Time Data")

plt.show()
```

The numerical integral gives an estimate of the total charge delivered during the measured time interval.

---

## 22.18 Complete Differentiation Example

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.array([0, 1, 2, 3, 4, 5])
V = np.array([0, 1, 4, 9, 16, 25])

# Numerical derivative
dVdx = np.gradient(V, x)

print(dVdx)

# Plot
plt.plot(x, V, marker="o", label="Original data")
plt.plot(x, dVdx, marker="s", label="Derivative")

plt.xlabel("x")
plt.ylabel("Value")
plt.title("Numerical Differentiation")
plt.legend()

plt.show()
```

---

## 22.19 Why Experimental Differentiation Can Be Sensitive to Noise

Differentiation is often more sensitive to measurement noise than integration.

Suppose the measured values are:

```text
1.00
1.05
0.98
1.10
1.02
```

Small measurement variations can produce relatively large changes in calculated slopes.

Therefore, when differentiating experimental data, it is important to:

- inspect the original data
- check measurement noise
- use appropriate spacing
- avoid interpreting isolated derivative values without context

This is particularly important for real experimental data.

---

## 22.20 Numerical Integration Can Also Be Affected by Data Quality

Integration depends on the measurements across the entire interval.

If one measurement is incorrect, the calculated area can be affected.

Therefore:

$$
\boxed{
\text{Good Numerical Calculation}
\text{ requires }
\text{Good Experimental Data}
}
$$

This connects Chapter 22 directly with the data-cleaning concepts from Chapter 18.

---

## 22.21 Choosing the Right Method

A simple guide is:

| Problem | Method |
|---|---|
| Area under a curve | Numerical integration |
| Total charge from current–time data | Integration |
| Accumulated quantity | Integration |
| Rate of change | Differentiation |
| Derivative from discrete measurements | `np.gradient()` |
| Integral of tabulated data | `scipy.integrate.trapezoid()` |
| Cumulative integral | `scipy.integrate.cumulative_trapezoid()` |

---

## 22.22 Key Points

- Numerical integration estimates accumulated quantities from numerical data.
- The trapezoidal rule is a simple and useful integration method.
- `scipy.integrate.trapezoid()` can integrate tabulated data.
- `scipy.integrate.cumulative_trapezoid()` gives cumulative integration.
- Numerical differentiation estimates rates of change.
- `np.gradient()` can estimate derivatives from discrete data.
- Units must always be checked.
- Experimental noise can strongly affect numerical differentiation.
- Data quality affects both integration and differentiation.
- Numerical results must be interpreted in the context of the experiment.

---

## 22.23 Quick Practice

### Practice 1

What does numerical integration calculate?

### Practice 2

For current $I(t)$, what physical quantity can be obtained from:

$$
Q=\int I(t)\,dt
$$

### Practice 3

Which SciPy function can calculate the integral of tabulated data using the trapezoidal method?

### Practice 4

Which NumPy function can estimate the derivative of discrete data?

### Practice 5

Why can experimental noise be especially problematic for numerical differentiation?

---

## 22.24 Hands-on Activity

Use the following current–time data:

```python
import numpy as np

t = np.array([0, 1, 2, 3, 4, 5])
I = np.array([0.0, 0.5, 1.0, 1.5, 1.2, 0.8])
```

Perform the following tasks:

1. Plot current against time.
2. Calculate the total charge using `trapezoid()`.
3. Calculate cumulative charge using `cumulative_trapezoid()`.
4. Plot cumulative charge against time.
5. Report the final charge with its unit.
6. Explain what the area under the current–time curve represents.

Then use:

```python
x = np.array([0, 1, 2, 3, 4, 5])
V = np.array([0, 1, 4, 9, 16, 25])
```

7. Calculate the numerical derivative using `np.gradient()`.
8. Plot the original data and derivative.
9. Explain what the derivative represents.

### Scientific Question

Why is the area under a current–time graph related to charge?

---

## 22.25 Chapter Summary

Numerical integration and differentiation allow us to extract additional scientific information from experimental data.

The basic workflow is:

$$
\boxed{
\text{Experimental Data}
\rightarrow
\text{Numerical Method}
\rightarrow
\text{Calculated Quantity}
\rightarrow
\text{Units}
\rightarrow
\text{Scientific Interpretation}
}
$$

In this chapter:

- integration was used to calculate accumulated quantities such as charge
- differentiation was used to estimate rates of change
- SciPy was used for numerical integration
- NumPy was used for numerical differentiation
- experimental plots were used to interpret the results

The next chapter focuses on **solving scientific equations numerically**, using SciPy to find solutions when analytical methods are difficult or inconvenient.
