# Chapter 19 — Introduction to SciPy

## 19.1 Introduction

Python provides many tools for scientific computing.

So far, we have used:

- Python for calculations
- NumPy for numerical arrays and mathematical operations
- pandas for scientific data handling
- Matplotlib for visualization

Another important library is **SciPy**.

SciPy provides ready-to-use scientific and numerical methods for problems such as:

- curve fitting
- interpolation
- numerical integration
- numerical differentiation
- solving equations
- optimization
- signal and data processing

A simple scientific workflow is:

$$
\boxed{
\text{Experimental Data}
\rightarrow
\text{NumPy/pandas}
\rightarrow
\text{SciPy Analysis}
\rightarrow
\text{Scientific Interpretation}
}
$$

SciPy is especially useful when a problem requires a numerical method rather than only basic arithmetic.

---

## 19.2 What Is SciPy?

**SciPy** stands for **Scientific Python**.

It is a Python library built on top of NumPy and provides specialized scientific algorithms.

NumPy mainly provides:

- arrays
- mathematical operations
- numerical calculations

SciPy provides higher-level scientific methods.

For example:

```python
import numpy as np
```

is commonly used for array calculations.

```python
from scipy import ...
```

is used when we need specialized scientific functions.

---

## 19.3 Installing SciPy

If SciPy is not already installed in the Python environment, it can be installed using:

```bash
conda install scipy
```

or:

```bash
pip install scipy
```

In the Python environment used for this course, check the installation using:

```python
import scipy

print(scipy.__version__)
```

If a version number is displayed, SciPy is available.

---

## 19.4 SciPy Modules

SciPy is organized into different modules.

Some important modules are:

| Module | Purpose |
|---|---|
| `scipy.optimize` | Optimization and curve fitting |
| `scipy.interpolate` | Interpolation |
| `scipy.integrate` | Numerical integration |
| `scipy.signal` | Signal processing |
| `scipy.linalg` | Linear algebra |
| `scipy.stats` | Statistical methods |
| `scipy.fft` | Fourier transforms |

In this course, we mainly use SciPy for scientific data analysis related to circuits and semiconductor devices.

---

## 19.5 SciPy and NumPy

It is useful to understand the relationship between NumPy and SciPy.

### NumPy

NumPy answers questions such as:

> How can I store and calculate with numerical data?

Example:

```python
import numpy as np

V = np.array([0.1, 0.2, 0.3, 0.4])

print(V * 2)
```

Output:

```text
[0.2 0.4 0.6 0.8]
```

### SciPy

SciPy answers questions such as:

> How can I apply a scientific numerical method to this data?

For example:

- find a fitted curve
- estimate a value between measurements
- calculate an integral
- solve an equation

Therefore:

$$
\boxed{
\text{NumPy} = \text{Numerical Foundation}
}
$$

and

$$
\boxed{
\text{SciPy} = \text{Scientific Numerical Tools}
}
$$

---

## 19.6 A Simple SciPy Example

Let us consider a simple equation:

$$
x^2-4=0
$$

The solutions are:

$$
x=2
$$

and

$$
x=-2
$$

Instead of solving it manually, SciPy can numerically find a root.

We can use `scipy.optimize.root_scalar`.

```python
from scipy.optimize import root_scalar

def equation(x):
    return x**2 - 4

result = root_scalar(equation, bracket=[0, 3])

print(result.root)
```

Output:

```text
2.0
```

Here:

```python
equation(x)
```

defines the equation.

```python
bracket=[0, 3]
```

tells SciPy that the required solution lies between 0 and 3.

---

## 19.7 What Is a Numerical Solution?

Many scientific equations cannot be solved conveniently using a simple formula.

For example:

$$
f(x)=0
$$

A numerical method searches for a value of $x$ for which:

$$
f(x)\approx0
$$

SciPy provides algorithms that perform this search efficiently.

This is useful in scientific problems involving:

- device equations
- circuit equations
- material properties
- physical models
- nonlinear relationships

---

## 19.8 Example: Finding a Voltage from a Model

Suppose a simplified model gives:

$$
I=V^2-1
$$

We want to find the voltage for which:

$$
I=0
$$

Therefore:

$$
V^2-1=0
$$

Use SciPy:

```python
from scipy.optimize import root_scalar

def current(V):
    return V**2 - 1

result = root_scalar(current, bracket=[0, 2])

print("Voltage =", result.root, "V")
```

Output:

```text
Voltage = 1.0 V
```

The important idea is not the particular equation.

The important idea is:

> SciPy can numerically solve scientific equations defined by Python functions.

---

## 19.9 Interpolation with SciPy

Suppose an experiment gives:

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.1 | 0.02 |
| 0.2 | 0.05 |
| 0.3 | 0.10 |
| 0.4 | 0.18 |

What is the approximate current at:

$$
V=0.25\text{ V}?
$$

There is no direct measurement at 0.25 V.

We can estimate it using interpolation.

SciPy provides interpolation methods through `scipy.interpolate`.

For example:

```python
import numpy as np
from scipy.interpolate import interp1d

V = np.array([0.1, 0.2, 0.3, 0.4])
I = np.array([0.02, 0.05, 0.10, 0.18])

f = interp1d(V, I)

I_025 = f(0.25)

print(I_025)
```

The result is an estimated current between the measured points.

Interpolation will be studied in detail in Chapter 21.

---

## 19.10 Numerical Integration

Integration is important in many scientific applications.

For example:

$$
A=\int_a^b f(x)\,dx
$$

may represent:

- area under a curve
- accumulated quantity
- charge from a current-time curve
- energy-related calculations

SciPy provides numerical integration tools through:

```python
scipy.integrate
```

For example:

```python
from scipy.integrate import quad

def f(x):
    return x**2

area, error = quad(f, 0, 2)

print(area)
```

Mathematically:

$$
\int_0^2 x^2\,dx
=
\frac{8}{3}
$$

The numerical result is approximately:

```text
2.6666666666666665
```

The second returned value represents an estimate of numerical error.

Numerical integration will be discussed further in Chapter 22.

---

## 19.11 Curve Fitting

Experimental data rarely lies exactly on a mathematical curve.

For example, a semiconductor experiment may produce:

```text
Voltage       Current
0.1           0.02
0.2           0.05
0.3           0.11
0.4           0.20
```

We may want to find a mathematical relationship between voltage and current.

SciPy provides curve-fitting tools.

The main function used later in this course is:

```python
scipy.optimize.curve_fit
```

Example:

```python
import numpy as np
from scipy.optimize import curve_fit

def model(x, a, b):
    return a*x + b

x = np.array([1, 2, 3, 4])
y = np.array([2.1, 4.0, 6.2, 8.1])

parameters, covariance = curve_fit(model, x, y)

print(parameters)
```

The returned parameters provide estimates of the model constants.

Curve fitting will be studied in detail in Chapter 20.

---

## 19.12 Signal and Data Processing

Experimental measurements may contain noise.

For example, a sensor may produce:

```text
2.01
2.05
1.98
2.20
2.02
2.01
```

The value `2.20` may be an unusual measurement.

SciPy provides signal-processing tools that can be used for tasks such as:

- smoothing
- filtering
- detecting peaks
- analyzing signals

These methods are available mainly through:

```python
scipy.signal
```

Signal and data processing will be discussed later in Chapter 24.

---

## 19.13 A Scientific Example

Consider an experiment measuring the response of a semiconductor device.

The data workflow may look like:

```text
Experimental Instrument
        ↓
      CSV File
        ↓
      pandas
        ↓
   Data Cleaning
        ↓
       NumPy
        ↓
       SciPy
        ↓
Curve Fit / Interpolation / Integration
        ↓
    Matplotlib
        ↓
Scientific Interpretation
```

Each library has a different role.

| Tool | Main role |
|---|---|
| pandas | Data handling |
| NumPy | Numerical arrays and calculations |
| SciPy | Scientific numerical methods |
| Matplotlib | Visualization |

Together, they form a powerful scientific computing workflow.

---

## 19.14 SciPy Does Not Replace Scientific Understanding

Suppose SciPy gives:

```text
2.43
```

The computer has produced a numerical result.

But the scientist must ask:

- What does 2.43 represent?
- What are its units?
- Is the result physically reasonable?
- What assumptions were made?
- How accurate is the result?
- Does it agree with the experimental data?

Therefore:

$$
\boxed{
\text{Numerical Result}
\neq
\text{Scientific Conclusion}
}
$$

A numerical result becomes scientifically useful only after interpretation.

---

## 19.15 Choosing the Right SciPy Tool

A simple decision guide is:

| Scientific problem | SciPy module |
|---|---|
| Fit a mathematical model | `scipy.optimize` |
| Find a root of an equation | `scipy.optimize` |
| Estimate values between measurements | `scipy.interpolate` |
| Calculate area numerically | `scipy.integrate` |
| Process noisy measurements | `scipy.signal` |
| Statistical analysis | `scipy.stats` |

This table will become useful as we move through Unit II.

---

## 19.16 Complete Small Example

Suppose we have experimental measurements:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.interpolate import interp1d

V = np.array([0.1, 0.2, 0.3, 0.4])
I = np.array([0.02, 0.05, 0.10, 0.18])

# Create interpolation function
model = interp1d(V, I)

# Estimate current at 0.25 V
I_est = model(0.25)

print("Estimated current =", I_est, "mA")

# Plot experimental data
plt.scatter(V, I)
plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Diode Experimental Data")
plt.show()
```

Here:

- NumPy stores the data.
- SciPy estimates the value between observations.
- Matplotlib displays the experimental measurements.

This is a simple example of combining scientific Python libraries.

---

## 19.17 Key Points

- SciPy is a scientific computing library for Python.
- SciPy works closely with NumPy.
- `scipy.optimize` is used for optimization, equation solving, and curve fitting.
- `scipy.interpolate` is used for interpolation.
- `scipy.integrate` is used for numerical integration.
- `scipy.signal` provides signal-processing methods.
- SciPy performs numerical methods; scientific interpretation remains the responsibility of the researcher.
- SciPy is particularly useful when experimental data requires mathematical or numerical analysis.

---

## 19.18 Quick Practice

### Practice 1

What is the main purpose of SciPy?

### Practice 2

Which SciPy module is commonly used for curve fitting?

### Practice 3

Which SciPy module is used for interpolation?

### Practice 4

Which module can be used for numerical integration?

### Practice 5

Why is scientific interpretation still required after obtaining a numerical result from SciPy?

---

## 19.19 Hands-on Activity

Use the following experimental data:

```python
import numpy as np

V = np.array([0.1, 0.2, 0.3, 0.4, 0.5])
I = np.array([0.02, 0.05, 0.10, 0.18, 0.30])
```

Perform the following tasks:

1. Import NumPy.
2. Import `interp1d` from `scipy.interpolate`.
3. Create an interpolation function.
4. Estimate the current at 0.25 V.
5. Estimate the current at 0.45 V.
6. Print both estimated values.
7. Plot the experimental measurements using Matplotlib.
8. Explain why the estimated values are not direct experimental measurements.

### Scientific Question

If the instrument did not measure current at 0.25 V, what does the interpolated value actually represent?

---

## 19.20 Chapter Summary

SciPy extends Python from basic numerical calculation to scientific numerical analysis.

The basic idea is:

$$
\boxed{
\text{Data}
\rightarrow
\text{Numerical Method}
\rightarrow
\text{Result}
\rightarrow
\text{Interpretation}
}
$$

In the following chapters, we will study the major SciPy methods required for this course:

- curve fitting
- interpolation
- numerical integration and differentiation
- solving scientific equations
- basic signal and data processing

These methods will later be applied directly to circuit and semiconductor device data.
