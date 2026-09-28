# Chapter 30 — Fitting, Goodness of Fit and Interpretation

## 30.1 Introduction

Scientific experiments often produce data points that do not lie exactly on a simple mathematical curve.

For example, a semiconductor experiment may produce voltage and current measurements such as:

```python
V = np.array([0.10, 0.15, 0.20, 0.25, 0.30,
              0.35, 0.40, 0.45, 0.50])

I = np.array([0.10, 0.18, 0.31, 0.52, 0.86,
              1.35, 2.05, 3.10, 4.55])
```

We may want to answer:

- What mathematical relationship describes the data?
- Which model fits the data better?
- How large are the residuals?
- How good is the fit?
- What do the fitted parameters mean physically?

This chapter brings together several ideas from Unit II:

$$
\boxed{
\text{Experimental Data}
\rightarrow
\text{Model}
\rightarrow
\text{Fit}
\rightarrow
\text{Residuals}
\rightarrow
\text{Goodness of Fit}
\rightarrow
\text{Interpretation}
}
$$

---

## 30.2 What Is Curve Fitting?

Curve fitting means finding the parameters of a mathematical model so that the model describes the observed data as closely as possible.

Suppose our model is:

$$
y=f(x,\theta)
$$

where $\theta$ represents one or more unknown parameters.

The experimental data provide values of $x$ and $y$.

Fitting estimates the parameters $\theta$.

For example, a straight-line model is:

$$
y=ax+b
$$

where:

- $a$ = slope
- $b$ = intercept

---

## 30.3 Why Do We Fit Experimental Data?

Experimental measurements usually contain:

- measurement noise
- random variation
- instrument limitations
- physical variation

Therefore, data points rarely lie perfectly on a theoretical curve.

A fitted model helps us:

1. summarize the relationship
2. estimate parameters
3. compare models
4. make predictions within the measured range
5. interpret physical behaviour

---

## 30.4 Linear Fitting

The simplest model is:

$$
y=ax+b
$$

For example, suppose voltage and current data approximately follow a linear relationship.

We can use NumPy:

```python
coefficients = np.polyfit(V, I, 1)

slope = coefficients[0]
intercept = coefficients[1]
```

The fitted values are:

```python
I_fit = np.polyval(coefficients, V)
```

The important point is that the computer estimates the values of $a$ and $b$ from the experimental data.

---

## 30.5 Plotting the Linear Fit

```python
import numpy as np
import matplotlib.pyplot as plt

V = np.array([0.10, 0.15, 0.20, 0.25, 0.30,
              0.35, 0.40, 0.45, 0.50])

I = np.array([0.10, 0.18, 0.31, 0.52, 0.86,
              1.35, 2.05, 3.10, 4.55])

coefficients = np.polyfit(V, I, 1)

slope = coefficients[0]
intercept = coefficients[1]

I_fit = np.polyval(coefficients, V)

plt.scatter(V, I, label="Experimental data")
plt.plot(V, I_fit, label="Linear fit")

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Experimental Data with Linear Fit")
plt.grid()
plt.legend()
plt.show()
```

```{figure} ../images/ch30/ch30_linear_fit.png
:name: ch30-linear-fit
:align: center

Experimental data with a linear fitted model.
```

The line gives the best straight-line description of the measured data according to the fitting method.

---

## 30.6 Interpreting the Linear Parameters

For:

$$
I=aV+b
$$

the slope is:

$$
a=\frac{\Delta I}{\Delta V}
$$

Its units are:

$$
\frac{\text{mA}}{\text{V}}
$$

The intercept is the predicted current when:

$$
V=0
$$

However, a fitted parameter should not automatically be interpreted as a physical constant.

The validity of that interpretation depends on whether the linear model is physically appropriate for the experiment.

---

## 30.7 What Is a Residual?

A residual measures the difference between an observed value and the corresponding fitted value.

$$
e_i=y_i-\hat{y}_i
$$

where:

- $y_i$ = observed value
- $\hat{y}_i$ = fitted value
- $e_i$ = residual

In Python:

```python
residual = I - I_fit
```

For example, if:

$$
I_{\text{observed}}=0.86\text{ mA}
$$

and:

$$
I_{\text{fitted}}=0.80\text{ mA}
$$

then:

$$
e=0.86-0.80
$$

so:

$$
e=0.06\text{ mA}
$$

---

## 30.8 Why Are Residuals Important?

A fitted curve can look visually reasonable but still fail to describe important patterns in the data.

Residuals help us examine the quality of the model.

A useful residual plot should ideally show:

- values scattered around zero
- no obvious systematic pattern
- no strong curve
- no large unexplained structure

A clear pattern in the residuals may indicate that the chosen model is not appropriate.

---

## 30.9 Sum of Squared Errors

One simple measure of overall fitting error is the **sum of squared errors**:

$$
SSE=\sum_{i=1}^{n}(y_i-\hat{y}_i)^2
$$

In Python:

```python
SSE = np.sum((I - I_fit)**2)

print("SSE:", SSE)
```

A smaller SSE means that the fitted values are, overall, closer to the observed values for the same dataset and response variable.

However, SSE should not be interpreted in isolation when comparing different datasets or models with different structures.

---

## 30.10 Exponential Fitting

Many semiconductor relationships are not linear.

A common teaching model is:

$$
I=ae^{bV}
$$

where:

- $a$ = scale parameter
- $b$ = growth parameter

Taking logarithms:

$$
\ln(I)=\ln(a)+bV
$$

This gives a linear relationship between $\ln(I)$ and $V$.

For the present classroom example, we can use this transformation to estimate the exponential parameters.

```python
logI = np.log(I)

coefficients = np.polyfit(V, logI, 1)

b = coefficients[0]
a = np.exp(coefficients[1])

print("a =", a)
print("b =", b)
```

The fitted model is then:

```python
I_fit = a * np.exp(b * V)
```

---

## 30.11 Plotting the Exponential Fit

```python
V_dense = np.linspace(V.min(), V.max(), 200)

I_fit_dense = a * np.exp(b * V_dense)

plt.scatter(V, I, label="Experimental data")
plt.plot(V_dense, I_fit_dense, label="Exponential fit")

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Experimental Data with Exponential Fit")
plt.grid()
plt.legend()
plt.show()
```

```{figure} ../images/ch30/ch30_exponential_fit.png
:name: ch30-exponential-fit
:align: center

Experimental data with an exponential fitted model.
```

The exponential model follows the increasing curvature of the dataset more closely than the straight-line model.

---

## 30.12 Comparing Two Models

Suppose we have:

### Linear model

$$
I=aV+b
$$

### Exponential model

$$
I=ae^{bV}
$$

We can calculate residuals for both.

```python
linear_residual = I - I_linear
exponential_residual = I - I_exp
```

Then compare their SSE values.

```python
SSE_linear = np.sum(linear_residual**2)
SSE_exponential = np.sum(exponential_residual**2)

print("Linear SSE:", SSE_linear)
print("Exponential SSE:", SSE_exponential)
```

For this teaching dataset:

- Linear SSE ≈ 2.55
- Exponential SSE ≈ 0.03

The exponential model therefore gives a much smaller SSE for this particular dataset.

This is a **model comparison for this dataset**, not a statement that exponential models are always better.

---

## 30.13 Residual Plot

```python
plt.axhline(0, linestyle="--")

plt.plot(
    V,
    linear_residual,
    marker="o",
    label="Linear residuals"
)

plt.plot(
    V,
    exponential_residual,
    marker="s",
    label="Exponential residuals"
)

plt.xlabel("Voltage (V)")
plt.ylabel("Residual (mA)")
plt.title("Residuals from Two Models")
plt.grid()
plt.legend()
plt.show()
```

```{figure} ../images/ch30/ch30_residual_comparison.png
:name: ch30-residual-comparison
:align: center

Comparison of residuals from linear and exponential models.
```

The residual plot helps us see whether the model systematically misses the data.

---

## 30.14 Coefficient of Determination: $R^2$

Another commonly used measure for a fitted model is the coefficient of determination:

$$
R^2
=
1-
\frac{\sum(y_i-\hat{y}_i)^2}
{\sum(y_i-\bar{y})^2}
$$

The numerator measures unexplained squared variation.

The denominator represents the total variation around the mean.

For the linear example:

```python
SSE = np.sum((I - I_linear)**2)

SST = np.sum((I - np.mean(I))**2)

R2 = 1 - SSE / SST

print("R²:", R2)
```

For this dataset, the linear fit gives approximately:

$$
R^2\approx0.80
$$

The exponential fit gives a value much closer to 1 for this teaching dataset.

---

## 30.15 What Does $R^2$ Mean?

For this introductory course, think of $R^2$ as describing how much of the observed variation is represented by the fitted model, under the assumptions of the calculation.

A value closer to 1 indicates a closer fit to the observed data.

But:

> A high $R^2$ does not automatically prove that a model is physically correct.

A model can fit the measured data well and still be physically inappropriate outside the conditions under which it was measured.

---

## 30.16 Observed vs Predicted Values

Another useful visualization compares:

- observed values
- predicted values

If predictions were perfect:

$$
\text{Predicted}=\text{Observed}
$$

The points would lie on:

$$
y=x
$$

```python
plt.scatter(I, I_exp)

mn = min(I.min(), I_exp.min())
mx = max(I.max(), I_exp.max())

plt.plot(
    [mn, mx],
    [mn, mx],
    linestyle="--"
)

plt.xlabel("Observed Current (mA)")
plt.ylabel("Predicted Current (mA)")
plt.title("Observed vs Predicted Values")
plt.grid()
plt.show()
```

```{figure} ../images/ch30/ch30_observed_vs_predicted.png
:name: ch30-observed-vs-predicted
:align: center

Observed and predicted values for the exponential model.
```

The closer the points are to the $y=x$ line, the closer the predictions are to the observations.

---

## 30.17 Good Fit Does Not Mean Correct Physics

This is one of the most important ideas in scientific data analysis.

Suppose two equations both fit an experimental dataset reasonably well.

A researcher should not select a model only because it gives the highest numerical fit measure.

The model should also be:

- physically meaningful
- appropriate for the device
- consistent with known behaviour
- valid over the experimental range
- supported by the experimental conditions

Therefore:

$$
\boxed{
\text{Good Numerical Fit}
\neq
\text{Automatically Correct Physical Model}
}
$$

---

## 30.18 Fitted Parameters Must Be Interpreted

Suppose a fitted model is:

$$
I=ae^{bV}
$$

The values of $a$ and $b$ are not just numbers.

They have mathematical and potentially physical meaning.

For example:

- $a$ controls the scale of the response
- $b$ controls how rapidly the response changes with voltage

But their physical interpretation depends on the model and the experiment.

A scientific report should therefore explain the parameter rather than simply listing it.

---

## 30.19 Linear Fit vs Exponential Fit

| Feature | Linear Model | Exponential Model |
|---|---|---|
| Equation | $y=ax+b$ | $y=ae^{bx}$ |
| Shape | Straight line | Curved |
| Parameters | slope, intercept | scale, growth parameter |
| Useful for | approximately linear data | rapidly increasing responses |
| Main checks | residuals, $R^2$ | residuals, fit quality, physical model |
| Physical interpretation | depends on experiment | depends on experiment |

The correct model depends on the scientific problem.

---

## 30.20 Interpolation vs Fitting

These concepts should not be confused.

### Interpolation

Estimates a value between measured data points.

### Fitting

Finds a mathematical model that describes the overall data.

For example:

If measurements exist at:

$$
V=0.2,\ 0.3,\ 0.4\text{ V}
$$

and we want an estimate at:

$$
V=0.25\text{ V}
$$

we can use interpolation.

If we want a mathematical relationship describing all the measured data, we use fitting.

---

## 30.21 Extrapolation Warning

A fitted model may work well inside the measured range but behave poorly outside it.

Suppose measurements were collected only between:

$$
0.10\leq V\leq0.50
$$

Using the fitted equation to predict current at:

$$
V=2.0\text{ V}
$$

would be an extrapolation.

The result may not be physically reliable.

Therefore:

> Always distinguish interpolation from extrapolation.

---

## 30.22 A Complete Fitting Workflow

For a semiconductor experiment, a useful workflow is:

### Step 1 — Collect data

Measure voltage and current.

### Step 2 — Inspect the data

Check:

- missing values
- units
- unusual observations
- measurement range

### Step 3 — Plot the data

Look at the shape before choosing a model.

### Step 4 — Select a physically reasonable model

For example:

$$
I=aV+b
$$

or:

$$
I=ae^{bV}
$$

### Step 5 — Fit the model

Estimate the parameters.

### Step 6 — Calculate residuals

$$
e_i=y_i-\hat{y}_i
$$

### Step 7 — Assess goodness of fit

Use appropriate measures such as:

- SSE
- $R^2$
- residual plots

### Step 8 — Interpret the parameters

Explain what the fitted values mean.

### Step 9 — Report limitations

State the measured range and important experimental conditions.

---

## 30.23 Common Mistakes

### Mistake 1: Choosing a model before looking at the data

Always plot the data first.

### Mistake 2: Assuming a high $R^2$ proves the theory

$R^2$ describes the fit to the data. It does not independently prove physical correctness.

### Mistake 3: Ignoring residuals

Two models may have similar overall fit measures but very different residual patterns.

### Mistake 4: Confusing interpolation and extrapolation

Predictions outside the measured range require additional caution.

### Mistake 5: Reporting parameters without interpretation

A fitted parameter should be explained in terms of the model and experiment.

### Mistake 6: Treating synthetic data as experimental evidence

The datasets in this chapter are teaching examples. Real experiments require appropriate uncertainty analysis and experimental documentation.

---

## 30.24 Key Points

- Curve fitting estimates model parameters from experimental data.
- A linear model has the form:

$$
y=ax+b
$$

- An exponential model can be written as:

$$
y=ae^{bx}
$$

- Residuals are:

$$
e_i=y_i-\hat{y}_i
$$

- SSE measures total squared fitting error.
- $R^2$ is a commonly used goodness-of-fit measure.
- Residual plots help identify systematic model errors.
- A high $R^2$ does not automatically prove physical correctness.
- Fitted parameters must be interpreted.
- Interpolation and extrapolation are different.
- A scientifically useful model should combine numerical fit with physical reasoning.

---

## 30.25 Quick Practice

1. What is curve fitting?
2. Write the equation of a linear model.
3. What is a residual?
4. Write the formula for SSE.
5. What does $R^2$ describe?
6. Why should residuals be examined?
7. What is the difference between interpolation and extrapolation?
8. Why does a high $R^2$ not automatically prove a physical model?
9. What information can a fitted parameter provide?
10. Why should the experimental range be reported?

---

## 30.26 Hands-on Activity

Use a semiconductor device dataset containing voltage and current.

Perform the following:

1. Enter the data into NumPy.
2. Plot the experimental data.
3. Fit a linear model.
4. Calculate the linear residuals.
5. Calculate SSE and $R^2$.
6. Fit an exponential model.
7. Calculate the exponential residuals.
8. Compare the two models.
9. Plot the residuals.
10. State which model describes the dataset more closely using the numerical and graphical evidence.
11. Explain whether the chosen model has a reasonable physical interpretation.
12. State the voltage range over which the model was evaluated.

---

## 30.27 Final Unit II Workflow

The major ideas from Unit II can now be connected:

$$
\boxed{
\text{Experimental Data}
\rightarrow
\text{Import}
\rightarrow
\text{Clean}
\rightarrow
\text{Analyze}
\rightarrow
\text{Visualize}
\rightarrow
\text{Fit}
\rightarrow
\text{Evaluate}
\rightarrow
\text{Interpret}
}
$$

Python provides the tools, but scientific reasoning determines how the results should be used.

---

## 30.28 Chapter Summary

In this final chapter, we used fitting and goodness-of-fit methods to turn experimental data into mathematical models.

The key lesson is:

> **Do not stop when Python gives you a fitted equation. Ask what the equation means, how well it describes the data, and whether it makes physical sense.**

This completes Unit II — **Data Analysis for Circuit and Semiconductor Device Applications**.
