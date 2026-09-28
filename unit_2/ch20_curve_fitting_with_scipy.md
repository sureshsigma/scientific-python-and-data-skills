# Chapter 20 — Curve Fitting with SciPy

## 20.1 Introduction

Experimental data rarely follows a perfect mathematical relationship.

For example, when measuring the current through a semiconductor device, the observed values may look like:

| Voltage (V) | Current (mA) |
|---:|---:|
| 1 | 2.1 |
| 2 | 4.2 |
| 3 | 5.9 |
| 4 | 8.1 |
| 5 | 10.2 |

The measurements contain small experimental variations.

We may want to find a mathematical equation that describes the overall relationship between the variables.

This process is called **curve fitting**.

The basic idea is:

$$
\boxed{
\text{Experimental Data}
\rightarrow
\text{Choose Model}
\rightarrow
\text{Fit Model}
\rightarrow
\text{Evaluate Fit}
\rightarrow
\text{Interpret}
}
$$

In this chapter, we use `scipy.optimize.curve_fit`.

---

## 20.2 What Is Curve Fitting?

Suppose we have measurements of two variables:

$$
(x_1,y_1),(x_2,y_2),\ldots,(x_n,y_n)
$$

We assume that the relationship can be approximately described by a mathematical model:

$$
y=f(x,\theta)
$$

where:

- $x$ is the independent variable
- $y$ is the measured dependent variable
- $f$ is the chosen mathematical model
- $\theta$ represents the model parameters

Curve fitting estimates the values of the parameters that make the model agree as closely as possible with the experimental data.

---

## 20.3 Why Do We Need a Model?

Suppose we measure voltage and current.

A table gives individual measurements:

```text
Voltage    Current
1          2.1
2          4.2
3          5.9
4          8.1
5          10.2
```

But we may want to describe the relationship using an equation.

For example:

$$
I=aV+b
$$

Now we have a mathematical model.

The goal is to estimate:

$$
a
$$

and

$$
b
$$

from the experimental data.

---

## 20.4 Linear Model

The simplest curve-fitting model is a straight line:

$$
y=ax+b
$$

where:

- $a$ is the slope
- $b$ is the intercept

For experimental data, the measured points will usually not lie exactly on the line.

The fitted line attempts to represent the overall trend.

---

## 20.5 Example Data

Consider:

```python
import numpy as np

V = np.array([1, 2, 3, 4, 5])
I = np.array([2.1, 4.2, 5.9, 8.1, 10.2])
```

We can visualize the measurements:

```python
import matplotlib.pyplot as plt

plt.scatter(V, I)

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Experimental Data")

plt.show()
```

The resulting plot is shown below.

```{figure} ../images/ch20/ch20_experimental_data.png
:alt: Experimental voltage-current data
:name: ch20-experimental-data

Experimental voltage-current measurements used for the curve-fitting example.
```

The points are approximately linear.

We can therefore try the model:

$$
I=aV+b
$$

---

## 20.6 Defining the Model in Python

The model can be written as a Python function:

```python
def linear_model(x, a, b):
    return a*x + b
```

Here:

- `x` is the independent variable
- `a` is the slope
- `b` is the intercept

For example, if:

```python
a = 2
b = 1
```

then:

$$
y=2x+1
$$

---

## 20.7 Using `curve_fit`

SciPy provides the function:

```python
curve_fit()
```

from:

```python
scipy.optimize
```

Complete example:

```python
import numpy as np
from scipy.optimize import curve_fit

V = np.array([1, 2, 3, 4, 5])
I = np.array([2.1, 4.2, 5.9, 8.1, 10.2])

def linear_model(x, a, b):
    return a*x + b

parameters, covariance = curve_fit(
    linear_model,
    V,
    I
)

a, b = parameters

print("Slope =", a)
print("Intercept =", b)
```

The estimated values of `a` and `b` describe the fitted line.

---

## 20.8 What Does `curve_fit()` Return?

The function returns two important objects:

```python
parameters, covariance = curve_fit(...)
```

### `parameters`

This contains the estimated model parameters.

For:

$$
y=ax+b
$$

we obtain:

```python
parameters[0]
```

for $a$, and

```python
parameters[1]
```

for $b$.

We can write:

```python
a, b = parameters
```

### `covariance`

The covariance matrix provides information related to the uncertainty of the estimated parameters.

For the introductory level of this course, we mainly focus on obtaining and interpreting the fitted parameters.

Parameter uncertainty will be connected with error analysis later.

---

## 20.9 Creating the Fitted Curve

After obtaining the parameters, we can calculate predicted values.

```python
I_fit = linear_model(V, a, b)

print(I_fit)
```

For a smoother plotted line, create more closely spaced voltage values:

```python
V_fit = np.linspace(V.min(), V.max(), 100)

I_fit = linear_model(V_fit, a, b)
```

Now plot both the measurements and fitted curve:

```python
import matplotlib.pyplot as plt

plt.scatter(V, I, label="Experimental data")
plt.plot(V_fit, I_fit, label="Fitted line")

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Linear Curve Fit")
plt.legend()

plt.show()
```

The resulting plot is shown below.

```{figure} ../images/ch20/ch20_linear_fit.png
:alt: Experimental data with fitted linear curve
:name: ch20-linear-fit

Experimental voltage-current measurements with the fitted linear model.
```

The points represent measurements.

The line represents the mathematical model fitted to the measurements.

---

## 20.10 Understanding the Fitted Parameters

For this example, the fitted model is approximately:

$$
I\approx2.02V+0.04
$$

The slope is approximately:

$$
a\approx2.02
$$

and the intercept is approximately:

$$
b\approx0.04
$$

The meaning of these values depends on the experiment and the units.

If $I$ is in mA and $V$ is in V, then the slope has units:

$$
\frac{\text{mA}}{\text{V}}
$$

This is an important scientific principle:

> A fitted parameter should be interpreted together with its units and physical meaning.

---

## 20.11 Residuals

A fitted model does not normally pass through every experimental point.

The difference between the observed value and the fitted value is called the **residual**.

For observation $i$:

$$
e_i=y_i-\hat{y}_i
$$

where:

- $y_i$ = observed value
- $\hat{y}_i$ = fitted value
- $e_i$ = residual

In Python:

```python
I_pred = linear_model(V, a, b)

residuals = I - I_pred

print(residuals)
```

A residual can be positive or negative.

---

## 20.12 Why Do We Look at Residuals?

Residuals help us understand how well the model represents the data.

For example:

```text
Observed    Fitted    Residual
2.10        2.06       0.04
4.20        4.08       0.12
5.90        6.10      -0.20
8.10        8.12      -0.02
10.20       10.14      0.06
```

If residuals are small, the fitted model is generally close to the observations.

We can also plot the residuals.

```python
plt.axhline(0)
plt.scatter(V, residuals)

plt.xlabel("Voltage (V)")
plt.ylabel("Residual (mA)")
plt.title("Residuals from Linear Fit")

plt.show()
```

```{figure} ../images/ch20/ch20_residuals.png
:alt: Residuals from linear fit
:name: ch20-residuals

Residuals showing the difference between measured and fitted current values.
```

A useful residual plot helps us see whether the errors are small and whether there is a systematic pattern.

But small residuals alone do not prove that the model is scientifically appropriate.

---

## 20.13 Sum of Squared Residuals

One common measure of overall fitting error is the sum of squared residuals:

$$
SSE=\sum_{i=1}^{n}(y_i-\hat{y}_i)^2
$$

where SSE means **Sum of Squared Errors**.

In Python:

```python
SSE = np.sum(residuals**2)

print("SSE =", SSE)
```

A smaller SSE indicates that the fitted values are closer to the observations for the same dataset and model comparison.

---

## 20.14 Fitting a Nonlinear Model

Not every scientific relationship is linear.

Suppose the model is:

$$
y=ae^{bx}
$$

This is an exponential model.

We can define it in Python:

```python
def exponential_model(x, a, b):
    return a * np.exp(b*x)
```

Then use:

```python
parameters, covariance = curve_fit(
    exponential_model,
    x,
    y
)
```

The fitted parameters are:

```python
a, b = parameters
```

The same general workflow is used:

$$
\text{Data}
\rightarrow
\text{Model}
\rightarrow
\text{curve\_fit}
\rightarrow
\text{Parameters}
\rightarrow
\text{Fitted Curve}
$$

A nonlinear model should be plotted and compared with the observations in the same way as a linear model.

---

## 20.15 Choosing a Model

Choosing the model is an important scientific decision.

Do not automatically fit a complicated equation just because Python can do it.

For example:

If the relationship appears approximately linear:

$$
y=ax+b
$$

a linear model may be appropriate.

If the scientific theory suggests exponential behavior:

$$
y=ae^{bx}
$$

an exponential model may be appropriate.

The model should be supported by:

- scientific theory
- experimental behavior
- the purpose of the analysis
- inspection of the data

---

## 20.16 Example: Semiconductor Response

Suppose an experiment measures the response of a device at different input voltages.

```python
V = np.array([1, 2, 3, 4, 5])
response = np.array([1.1, 2.0, 3.2, 4.1, 5.1])
```

First plot the data:

```python
plt.scatter(V, response)

plt.xlabel("Voltage (V)")
plt.ylabel("Device Response")
plt.show()
```

The data appears approximately linear.

We can fit:

$$
R=aV+b
$$

using:

```python
def linear_model(x, a, b):
    return a*x + b

parameters, covariance = curve_fit(
    linear_model,
    V,
    response
)

a, b = parameters

print("a =", a)
print("b =", b)
```

The fitted equation is then:

$$
\hat{R}=aV+b
$$

This equation can be used to describe the measured trend within the range of the experiment.

---

## 20.17 Do Not Extrapolate Carelessly

Suppose experimental data covers:

$$
0\leq x\leq5
$$

and we fit a model.

Using the fitted model to estimate:

$$
x=3
$$

is interpolation because 3 lies within the measured range.

Using it to estimate:

$$
x=20
$$

is extrapolation.

Extrapolation can be unreliable because the physical relationship may change outside the measured range.

Therefore:

> A fitted equation should not automatically be assumed valid outside the experimental range.

---

## 20.18 Complete Example

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit

# Experimental data
V = np.array([1, 2, 3, 4, 5])
I = np.array([2.1, 4.2, 5.9, 8.1, 10.2])

# Define model
def linear_model(x, a, b):
    return a*x + b

# Fit model
parameters, covariance = curve_fit(
    linear_model,
    V,
    I
)

a, b = parameters

# Predicted values
I_pred = linear_model(V, a, b)

# Residuals
residuals = I - I_pred

# Smooth fitted curve
V_fit = np.linspace(V.min(), V.max(), 100)
I_fit = linear_model(V_fit, a, b)

# Print results
print("Slope =", a)
print("Intercept =", b)
print("Residuals =", residuals)

# Plot
plt.scatter(V, I, label="Experimental data")
plt.plot(V_fit, I_fit, label="Fitted line")

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Experimental Data and Linear Fit")
plt.legend()

plt.show()
```

This example demonstrates the complete basic curve-fitting workflow.

---

## 20.19 Scientific Interpretation

Suppose the fitted equation is:

$$
I\approx2.02V+0.04
$$

We should not simply report the equation.

We should explain what it means.

For example:

> The experimental current shows an approximately linear increase with voltage over the measured range. The fitted slope describes the average change in current per unit change in voltage.

This is a scientific interpretation.

The exact interpretation depends on the experiment.

---

## 20.20 Important Distinction: Fit vs Theory

A fitted equation describes the observed data.

It does not automatically prove that the equation is the underlying physical law.

For example:

$$
y=ax+b
$$

may fit a small range of experimental observations well.

That does not necessarily mean the physical system is fundamentally linear at all values of $x$.

Therefore:

$$
\boxed{
\text{Good Fit}
\neq
\text{Proof of Physical Law}
}
$$

The fitted model should be interpreted using scientific knowledge.

---

## 20.21 Key Points

- Curve fitting finds model parameters that describe experimental data.
- `scipy.optimize.curve_fit` is a convenient tool for fitting mathematical models.
- A model is written as a Python function.
- For a linear model:

$$
y=ax+b
$$

- The fitted parameters are returned by `curve_fit()`.
- Residuals measure the difference between observed and fitted values.
- SSE is the sum of squared residuals.
- Nonlinear models can also be fitted.
- Model selection should be based on scientific reasoning, not only numerical fit.
- Fitted models should not be extrapolated carelessly.
- A good numerical fit does not automatically prove a physical law.

---

## 20.22 Quick Practice

### Practice 1

What is meant by curve fitting?

### Practice 2

For the model

$$
y=ax+b
$$

what do $a$ and $b$ represent?

### Practice 3

Which SciPy function is used in this chapter for curve fitting?

### Practice 4

Define the residual:

$$
e_i=y_i-\hat{y}_i
$$

### Practice 5

Why should a fitted model not automatically be used far outside the experimental range?

---

## 20.23 Hands-on Activity

Use the following experimental data:

```python
import numpy as np

V = np.array([1, 2, 3, 4, 5, 6])
I = np.array([2.1, 4.0, 6.2, 7.9, 10.1, 12.2])
```

Perform the following tasks:

1. Plot the experimental data.
2. Define a linear model:

$$
I=aV+b
$$

3. Fit the model using `curve_fit`.
4. Print the estimated slope and intercept.
5. Calculate predicted current values.
6. Calculate residuals.
7. Calculate SSE.
8. Plot the experimental points and fitted line.
9. Write the fitted equation.
10. Explain whether the data appears approximately linear.

### Scientific Question

What does the fitted slope tell you about the relationship between voltage and current in this dataset?

---

## 20.24 Chapter Summary

Curve fitting provides a way to represent experimental observations using a mathematical model.

The basic workflow is:

$$
\boxed{
\text{Experimental Data}
\rightarrow
\text{Choose Model}
\rightarrow
\text{Fit Parameters}
\rightarrow
\text{Calculate Predictions}
\rightarrow
\text{Check Residuals}
\rightarrow
\text{Interpret}
}
$$

In this chapter, `scipy.optimize.curve_fit` was introduced for linear and nonlinear models.

The next chapter focuses on **interpolation**, where we estimate values between known experimental observations.
