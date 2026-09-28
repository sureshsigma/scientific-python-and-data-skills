# Chapter 21 — Interpolation

## 21.1 Introduction

Experimental measurements are usually available only at selected values.

For example, suppose a diode experiment gives:

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.10 | 0.02 |
| 0.20 | 0.05 |
| 0.30 | 0.10 |
| 0.40 | 0.18 |

But suppose we want to know the approximate current at:

$$
V=0.25\text{ V}
$$

There is no direct measurement at 0.25 V.

We can estimate the value from the measurements around it.

This process is called **interpolation**.

The basic idea is:

$$
\boxed{
\text{Known Measurements}
\rightarrow
\text{Interpolation Method}
\rightarrow
\text{Estimated Value}
}
$$

---

## 21.2 What Is Interpolation?

Interpolation is the process of estimating a value **between known observations**.

Suppose we know:

$$
(x_1,y_1)
$$

and

$$
(x_2,y_2)
$$

and want to estimate $y$ for a value of $x$ between $x_1$ and $x_2$.

For example:

$$
x_1=0.2,\qquad y_1=0.05
$$

and

$$
x_2=0.3,\qquad y_2=0.10
$$

We want the approximate value at:

$$
x=0.25
$$

Since 0.25 lies between 0.20 and 0.30, this is interpolation.

---

## 21.3 Interpolation vs Extrapolation

This distinction is very important.

### Interpolation

Estimating a value **inside the range of measured data**.

For example:

Measured range:

$$
0.1\leq V\leq0.4
$$

Estimating at:

$$
V=0.25
$$

is interpolation.

### Extrapolation

Estimating a value **outside the measured range**.

For example, estimating at:

$$
V=0.6
$$

using measurements only from 0.1 V to 0.4 V is extrapolation.

Therefore:

$$
\boxed{
\text{Interpolation: Inside Data Range}
}
$$

$$
\boxed{
\text{Extrapolation: Outside Data Range}
}
$$

Extrapolation generally requires greater caution.

---

## 21.4 Linear Interpolation

The simplest interpolation method is **linear interpolation**.

Suppose:

$$
(x_1,y_1)
$$

and

$$
(x_2,y_2)
$$

are two known points.

The estimated value at $x$ is:

$$
y
=
y_1+
\frac{x-x_1}{x_2-x_1}
(y_2-y_1)
$$

This assumes that the relationship between the two known points is approximately linear.

---

## 21.5 Step-by-Step Example

Consider the diode measurements:

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.20 | 0.05 |
| 0.30 | 0.10 |

We want the current at:

$$
V=0.25\text{ V}
$$

Here:

$$
x_1=0.20,\quad y_1=0.05
$$

$$
x_2=0.30,\quad y_2=0.10
$$

and:

$$
x=0.25
$$

Using the linear interpolation formula:

$$
I
=
0.05+
\frac{0.25-0.20}{0.30-0.20}
(0.10-0.05)
$$

First:

$$
0.25-0.20=0.05
$$

and:

$$
0.30-0.20=0.10
$$

Therefore:

$$
\frac{0.05}{0.10}=0.5
$$

So:

$$
I
=
0.05+0.5(0.05)
$$

$$
I=0.075\text{ mA}
$$

Therefore, the estimated current is:

$$
\boxed{I\approx0.075\text{ mA}}
$$

This is an **estimated value**, not a direct experimental measurement.

---

## 21.6 Using SciPy for Interpolation

SciPy provides interpolation tools through:

```python
scipy.interpolate
```

One commonly used function is:

```python
interp1d
```

Import it using:

```python
from scipy.interpolate import interp1d
```

---

## 21.7 Creating the Experimental Dataset

```python
import numpy as np

V = np.array([0.1, 0.2, 0.3, 0.4])
I = np.array([0.02, 0.05, 0.10, 0.18])
```

Here:

- `V` contains voltage measurements.
- `I` contains current measurements.

The values are paired observations.

---

## 21.8 Creating an Interpolation Function

```python
from scipy.interpolate import interp1d

f = interp1d(V, I)
```

The object `f` represents the interpolation function.

We can now estimate current at a new voltage.

```python
I_025 = f(0.25)

print(I_025)
```

The result is approximately:

```text
0.075
```

So:

$$
I(0.25)\approx0.075\text{ mA}
$$

---

## 21.9 Plotting the Interpolation

We can visualize both the measured observations and the interpolated relationship.

```python
import matplotlib.pyplot as plt

V_smooth = np.linspace(0.1, 0.4, 200)
I_smooth = f(V_smooth)

plt.scatter(V, I, label="Experimental data")
plt.plot(V_smooth, I_smooth, label="Interpolated curve")

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Interpolation of Diode Experimental Data")
plt.legend()

plt.show()
```

The resulting figure is:

```{figure} ../images/ch21/ch21_linear_interpolation.png
:alt: Diode experimental data with linear interpolation
:name: ch21-linear-interpolation

Experimental diode measurements and the interpolated relationship between them.
```

The points are measured values.

The line represents values estimated between those measurements.

---

## 21.10 Highlighting an Estimated Value

We can specifically highlight the estimated value at 0.25 V.

```python
V_target = 0.25
I_target = f(V_target)

print("Estimated current =", I_target)
```

We can plot the estimated point:

```python
plt.scatter(V, I, label="Measured data")
plt.plot(V_smooth, I_smooth, label="Interpolated curve")

plt.scatter(
    V_target,
    I_target,
    label="Estimated point"
)

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Estimating Current at 0.25 V")
plt.legend()

plt.show()
```

```{figure} ../images/ch21/ch21_interpolated_point.png
:alt: Interpolated point at 0.25 volts
:name: ch21-interpolated-point

The current at 0.25 V is estimated from neighboring measurements.
```

The important point is that the value at 0.25 V was not directly measured.

It was estimated from the available observations.

---

## 21.11 Why Interpolation Is Useful

Interpolation is useful when experimental measurements are available only at selected points.

Applications include:

- diode I–V characteristics
- sensor calibration
- temperature measurements
- semiconductor device measurements
- material-property tables
- experimental calibration curves
- lookup tables

For example, if a device was measured at:

$$
V=0.1,\ 0.2,\ 0.3,\ 0.4\text{ V}
$$

we may need an approximate response at:

$$
V=0.25\text{ V}
$$

Interpolation provides a numerical estimate.

---

## 21.12 Different Interpolation Methods

SciPy provides several interpolation methods.

Some commonly encountered methods are:

- linear interpolation
- nearest-neighbor interpolation
- cubic interpolation

For example:

```python
f_linear = interp1d(V, I, kind="linear")
```

Nearest-neighbor interpolation:

```python
f_nearest = interp1d(V, I, kind="nearest")
```

Cubic interpolation:

```python
f_cubic = interp1d(V, I, kind="cubic")
```

The method should be selected according to the data and the scientific purpose.

---

## 21.13 Comparing Interpolation Methods

We can compare two simple methods:

```python
f_linear = interp1d(V, I, kind="linear")
f_nearest = interp1d(V, I, kind="nearest")

V_target = 0.25

print("Linear =", f_linear(V_target))
print("Nearest =", f_nearest(V_target))
```

The two methods may produce different estimates.

A visualization helps explain the difference:

```{figure} ../images/ch21/ch21_interpolation_methods.png
:alt: Comparison of interpolation methods
:name: ch21-interpolation-methods

Illustration of different interpolation approaches for the same experimental data.
```

For scientific experimental data, the choice of interpolation method should not be made only because one method produces a smoother curve.

---

## 21.14 Linear Interpolation Is Often a Good Starting Point

For classroom scientific data, linear interpolation is an excellent starting point because it is:

- simple
- easy to understand
- easy to calculate manually
- easy to implement in Python
- appropriate for many closely spaced measurements

However, it assumes that the relationship between neighboring measurements is approximately linear.

If the underlying physical relationship is strongly curved, another method may be more appropriate.

---

## 21.15 Interpolation Does Not Create New Experimental Information

Suppose the experiment measured:

```text
0.20 V → 0.05 mA
0.30 V → 0.10 mA
```

and interpolation gives:

```text
0.25 V → 0.075 mA
```

The 0.075 mA value is not a new measurement.

It is a mathematical estimate based on existing measurements.

Therefore:

$$
\boxed{
\text{Interpolated Value}
=
\text{Estimated Value}
}
$$

not:

$$
\boxed{
\text{Interpolated Value}
=
\text{New Experimental Measurement}
}
$$

This distinction is important when reporting scientific results.

---

## 21.16 Interpolation and Experimental Error

Experimental measurements contain uncertainty.

Suppose:

$$
I_1=0.05\text{ mA}
$$

and:

$$
I_2=0.10\text{ mA}
$$

Both measurements may contain experimental error.

The interpolated value also depends on those measurements.

Therefore, interpolation should not be interpreted as producing an exact value.

A useful statement is:

> The interpolated value is an estimate based on the available experimental measurements and the selected interpolation method.

Detailed uncertainty analysis is covered later in Chapter 29.

---

## 21.17 Complete Example

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.interpolate import interp1d

# Experimental data
V = np.array([0.1, 0.2, 0.3, 0.4])
I = np.array([0.02, 0.05, 0.10, 0.18])

# Create interpolation function
f = interp1d(V, I)

# Estimate current at 0.25 V
V_target = 0.25
I_target = f(V_target)

print("Estimated current =", I_target, "mA")

# Create smooth values for plotting
V_smooth = np.linspace(V.min(), V.max(), 200)
I_smooth = f(V_smooth)

# Plot
plt.scatter(V, I, label="Experimental data")
plt.plot(V_smooth, I_smooth, label="Interpolated curve")
plt.scatter(
    V_target,
    I_target,
    label="Estimated point"
)

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Diode Data Interpolation")
plt.legend()

plt.show()
```

This example follows the complete workflow:

$$
\boxed{
\text{Experimental Data}
\rightarrow
\text{Interpolation Function}
\rightarrow
\text{Estimate}
\rightarrow
\text{Plot}
\rightarrow
\text{Interpret}
}
$$

---

## 21.18 Important Scientific Caution

Interpolation works within the measured range, but the estimated value depends on the assumption made by the interpolation method.

For example, linear interpolation assumes approximately linear behavior between neighboring points.

If the actual physical relationship is strongly nonlinear, the estimate may differ from the true value.

Therefore:

> Always consider the physical behavior of the system before choosing an interpolation method.

---

## 21.19 Key Points

- Interpolation estimates values between known observations.
- SciPy provides interpolation tools through `scipy.interpolate`.
- `interp1d` can be used for one-dimensional interpolation.
- Linear interpolation is simple and useful for many experimental datasets.
- Interpolated values are estimates, not new measurements.
- Interpolation should not be confused with extrapolation.
- Extrapolation estimates values outside the measured range and requires greater caution.
- Different interpolation methods can produce different results.
- The interpolation method should be consistent with the data and scientific problem.
- Experimental uncertainty also affects interpolated values.

---

## 21.20 Quick Practice

### Practice 1

What is interpolation?

### Practice 2

What is the difference between interpolation and extrapolation?

### Practice 3

For the two observations:

$$
(0.20,0.05)
$$

and

$$
(0.30,0.10)
$$

estimate the value of $y$ at $x=0.25$ using linear interpolation.

### Practice 4

Which SciPy module is used for interpolation?

### Practice 5

Why is an interpolated value not considered a new experimental measurement?

---

## 21.21 Hands-on Activity

Use the following experimental data:

```python
import numpy as np

V = np.array([0.1, 0.2, 0.3, 0.4, 0.5])
I = np.array([0.02, 0.05, 0.10, 0.18, 0.30])
```

Perform the following tasks:

1. Import `interp1d`.
2. Create a linear interpolation function.
3. Estimate current at 0.25 V.
4. Estimate current at 0.35 V.
5. Estimate current at 0.45 V.
6. Plot the measured data.
7. Plot the interpolated curve.
8. Mark one estimated value on the graph.
9. Explain which values are measured and which are estimated.
10. Try nearest-neighbor interpolation and compare the result.

### Scientific Question

Why is it reasonable to estimate the current at 0.25 V from measurements at 0.20 V and 0.30 V, but more difficult to estimate the current at 1.0 V?

---

## 21.22 Chapter Summary

Interpolation allows us to estimate values between known experimental observations.

The basic workflow is:

$$
\boxed{
\text{Known Data}
\rightarrow
\text{Choose Method}
\rightarrow
\text{Estimate}
\rightarrow
\text{Visualize}
\rightarrow
\text{Interpret}
}
$$

In this chapter, we used SciPy's interpolation tools to estimate semiconductor device measurements between experimentally observed values.

The next chapter focuses on **numerical integration and differentiation**, where experimental data will be used for numerical calculations involving rates, accumulated quantities, and areas under curves.
