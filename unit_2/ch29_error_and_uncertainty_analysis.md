# Chapter 29 — Error and Uncertainty Analysis

## 29.1 Introduction

Experimental measurements are never perfectly exact.

When we measure voltage, current, resistance, temperature, or any semiconductor parameter, the measured value may differ slightly from the true or accepted value.

For example, suppose a voltage is measured several times:

$$
1.02,\ 1.01,\ 1.03,\ 1.00,\ 1.04\text{ V}
$$

The values are close, but they are not identical.

This chapter introduces simple methods for describing this variation.

We will study:

- mean
- deviation
- range
- standard deviation
- percentage error
- uncertainty
- error bars
- accuracy and precision
- uncertainty in calculated quantities

The main idea is:

$$
\text{Measurement}
\rightarrow
\text{Variation}
\rightarrow
\text{Quantify}
\rightarrow
\text{Report}
$$

---

## 29.2 Why Do Measurements Vary?

Experimental measurements can vary because of:

- instrument resolution
- electrical noise
- temperature changes
- environmental conditions
- calibration
- sample variation
- human reading error
- fluctuations in the experimental setup

Therefore, a scientific result should not always be reported as a single number without any indication of its reliability.

---

## 29.3 Repeated Measurements

Suppose we measure a voltage eight times.

```python
import numpy as np

V = np.array([
    1.02, 1.01, 1.03, 1.00,
    1.04, 1.02, 1.01, 1.03
])
```

We can visualize the measurements.

```python
import matplotlib.pyplot as plt

plt.plot(range(1, len(V)+1), V, marker="o")
plt.xlabel("Measurement Number")
plt.ylabel("Voltage (V)")
plt.title("Repeated Voltage Measurements")
plt.grid()
plt.show()
```

```{figure} ../images/ch29/ch29_repeated_measurements.png
:name: ch29-repeated-measurements
:align: center

Repeated measurements of voltage.
```

The measurements are close to each other, but there is some variation.

---

## 29.4 Mean

The arithmetic mean is:

$$
\bar{x}=
\frac{x_1+x_2+\cdots+x_n}{n}
$$

In Python:

```python
mean_V = np.mean(V)

print("Mean voltage:", mean_V, "V")
```

For the dataset:

$$
\bar{V}=1.02\text{ V}
$$

The mean gives a representative value for the repeated measurements.

---

## 29.5 Deviation from the Mean

For each measurement:

$$
d_i=x_i-\bar{x}
$$

This tells us how far an individual observation is from the mean.

In Python:

```python
deviation = V - mean_V

print(deviation)
```

For example, if:

$$
V=1.04\text{ V}
$$

and:

$$
\bar{V}=1.02\text{ V}
$$

then:

$$
d=1.04-1.02=0.02\text{ V}
$$

---

## 29.6 Range

The range is a simple measure of the spread of measurements.

$$
\text{Range}
=
x_{\max}-x_{\min}
$$

In Python:

```python
value_range = np.max(V) - np.min(V)

print("Range:", value_range, "V")
```

For this dataset:

$$
\text{Range}=1.04-1.00
$$

so:

$$
\text{Range}=0.04\text{ V}
$$

The range is easy to calculate, but it uses only the two extreme observations.

---

## 29.7 Standard Deviation

Standard deviation provides a more useful description of the spread of repeated measurements.

For a sample of measurements, we commonly use:

$$
s=
\sqrt{
\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}
{n-1}
}
$$

In NumPy:

```python
std_V = np.std(V, ddof=1)

print("Standard deviation:", std_V, "V")
```

The argument:

```python
ddof=1
```

means that NumPy calculates the **sample standard deviation**.

---

## 29.8 What Does Standard Deviation Tell Us?

A small standard deviation means the repeated measurements are relatively close to each other.

A larger standard deviation means the measurements are more spread out.

Therefore, standard deviation helps describe **measurement variability**.

It does not automatically tell us whether the measurements are close to the true value.

That distinction leads to the concepts of accuracy and precision.

---

## 29.9 Accuracy and Precision

### Accuracy

Accuracy refers to how close a measurement is to the accepted or reference value.

### Precision

Precision refers to how close repeated measurements are to each other.

These are different concepts.

A set of measurements can be:

- accurate and precise
- accurate but less precise
- precise but inaccurate
- neither accurate nor precise

```{figure} ../images/ch29/ch29_accuracy_precision.png
:name: ch29-accuracy-precision
:align: center

Illustration of measurement consistency and closeness to a reference value.
```

For scientific data analysis, it is important not to use the words **accuracy** and **precision** as if they mean the same thing.

---

## 29.10 Percentage Error

Suppose an experimental value is compared with an accepted value.

Percentage error is:

$$
\text{Percentage Error}
=
\left|
\frac{x_{\text{experimental}}-x_{\text{accepted}}}
{x_{\text{accepted}}}
\right|
\times100
$$

### Example

Suppose:

$$
x_{\text{accepted}}=1.00\text{ V}
$$

and:

$$
x_{\text{experimental}}=1.02\text{ V}
$$

Then:

$$
\text{Percentage Error}
=
\left|
\frac{1.02-1.00}{1.00}
\right|
\times100
$$

Therefore:

$$
\text{Percentage Error}=2\%
$$

---

## 29.11 Python Calculation of Percentage Error

```python
accepted = 1.00
experimental = 1.02

percentage_error = (
    abs(experimental - accepted)
    / accepted
) * 100

print("Percentage error:", percentage_error, "%")
```

Percentage error is useful when comparing an experimental result with:

- a theoretical value
- a standard value
- a calibrated value
- a value reported in a reference source

---

## 29.12 Experimental Uncertainty

Uncertainty describes the range within which a measured value is expected to lie.

For example, a measurement might be reported as:

$$
V=(1.02\pm0.02)\text{ V}
$$

This communicates more information than simply:

$$
V=1.02\text{ V}
$$

The uncertainty could arise from the instrument specification, repeated measurements, or another justified method.

The method used to estimate uncertainty should always be stated when reporting scientific results.

---

## 29.13 Instrument Resolution

Suppose a digital multimeter displays voltage to two decimal places:

$$
1.02\text{ V}
$$

The displayed resolution is approximately:

$$
0.01\text{ V}
$$

However, **resolution is not necessarily the same as measurement uncertainty**.

For example, an instrument may display values to 0.01 V but have a larger manufacturer-specified measurement uncertainty.

Therefore:

> Do not automatically report the last displayed digit as the total experimental uncertainty.

---

## 29.14 Error Bars

Error bars provide a visual representation of uncertainty.

Suppose we measure diode current at different voltages.

```python
V = np.array([0.10, 0.20, 0.30, 0.40, 0.50])

I = np.array([
    0.12, 0.45, 1.10, 2.40, 4.80
])

uncertainty = np.array([
    0.02, 0.03, 0.04, 0.06, 0.08
])
```

We can plot the data with error bars.

```python
plt.errorbar(
    V,
    I,
    yerr=uncertainty,
    fmt="o-",
    capsize=4
)

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Diode Current with Measurement Uncertainty")
plt.grid()
plt.show()
```

```{figure} ../images/ch29/ch29_error_bars_diode.png
:name: ch29-error-bars-diode
:align: center

Diode current measurements with illustrative uncertainty bars.
```

The vertical error bars show the assumed uncertainty associated with each current measurement.

---

## 29.15 Why Error Bars Are Useful

Compare two experimental datasets.

If two values are close but their uncertainty intervals are large, it may be difficult to distinguish them confidently.

Error bars therefore help us visually communicate the reliability or variability of measurements.

However, an error bar does not have a universal meaning.

It might represent:

- instrument uncertainty
- standard deviation
- standard error
- confidence interval
- another defined uncertainty measure

Therefore, a scientific graph should state what the error bars represent.

---

## 29.16 Standard Deviation vs Standard Error

These two quantities are often confused.

### Standard deviation

Describes the spread of individual observations.

### Standard error of the mean

Describes the uncertainty associated with the estimated mean.

A common expression is:

$$
SE=\frac{s}{\sqrt{n}}
$$

where:

- $s$ = sample standard deviation
- $n$ = number of observations

In Python:

```python
std = np.std(V, ddof=1)

n = len(V)

SE = std / np.sqrt(n)

print("Standard deviation:", std)
print("Standard error:", SE)
```

The two quantities answer different questions.

---

## 29.17 Error in a Calculated Quantity

Often, we do not directly measure the quantity we want.

Instead, we calculate it.

For example:

$$
\rho=R\frac{A}{L}
$$

Here resistivity is calculated from measurements of:

- resistance $R$
- length $L$
- area $A$

Therefore, uncertainty in these measurements can affect the calculated resistivity.

At an introductory level, it is enough to understand the principle:

> Uncertainty in input measurements can produce uncertainty in the calculated result.

A more detailed uncertainty-propagation treatment can be introduced later if required.

---

## 29.18 Simple Percentage Uncertainty

For some classroom calculations, a simple approximate approach is to consider percentage uncertainties.

For example, if:

$$
R=100\pm2\ \Omega
$$

then the percentage uncertainty in resistance is:

$$
\frac{2}{100}\times100=2\%
$$

Similarly, if:

$$
L=5.00\pm0.05\text{ mm}
$$

then:

$$
\text{Percentage uncertainty}
=
\frac{0.05}{5.00}\times100
=
1\%
$$

The exact propagation rule depends on how the quantities are combined.

---

## 29.19 Uncertainty in a Simple Product

Suppose:

$$
Q=AB
$$

and the relative uncertainties are small.

A commonly used introductory approximation is:

$$
\frac{\Delta Q}{Q}
\approx
\frac{\Delta A}{A}
+
\frac{\Delta B}{B}
$$

For example, if:

$$
A=100\pm2
$$

and:

$$
B=5.0\pm0.1
$$

then the percentage uncertainties are:

$$
2\%
$$

and:

$$
2\%
$$

respectively.

The approximate percentage uncertainty in $Q$ is therefore:

$$
2\%+2\%=4\%
$$

This is a simple conservative classroom rule. More formal uncertainty propagation can use root-sum-of-squares methods when the uncertainties are independent.

---

## 29.20 Reporting Experimental Results

A good scientific result should include:

1. measured or calculated value
2. unit
3. uncertainty when appropriate
4. method used
5. relevant conditions

For example:

> The measured voltage was $1.02\pm0.02$ V based on repeated measurements.

Or:

> The calculated resistivity was $0.025\ \Omega m$ using the measured resistance, sample length, and cross-sectional area.

This is more informative than simply writing:

> Voltage = 1.02

---

## 29.21 Example: Semiconductor Resistance Measurement

Suppose five measurements of resistance are:

```python
R = np.array([98, 101, 100, 102, 99])
```

Calculate the mean:

```python
mean_R = np.mean(R)
```

Calculate sample standard deviation:

```python
std_R = np.std(R, ddof=1)
```

Report:

```python
print("Mean resistance:", mean_R, "ohm")
print("Standard deviation:", std_R, "ohm")
```

The result can be reported approximately as:

$$
R=\bar{R}\pm s
$$

provided that this reporting convention is explicitly intended.

---

## 29.22 Scientific Interpretation

Suppose the result is approximately:

$$
R=(100\pm2)\ \Omega
$$

A suitable interpretation is:

> The repeated resistance measurements have a mean of approximately 100 Ω and show a spread of about 2 Ω as described by the sample standard deviation.

Notice that the statement explains what the uncertainty or spread represents.

---

## 29.23 Important Distinction: Error vs Uncertainty

In everyday language, "error" often means "mistake."

In experimental science, **measurement error** has a more specific meaning: the difference between a measured value and the true or reference value.

Uncertainty describes the lack of exact knowledge about the measurement.

Therefore, avoid saying:

> The measurement has an error of 0.02 V

unless you have clearly defined what "error" means.

If the quantity comes from repeated measurements, it may be more appropriate to say:

> The sample standard deviation is 0.02 V.

Or, if justified:

> The estimated measurement uncertainty is ±0.02 V.

---

## 29.24 Common Mistakes

### Mistake 1: Reporting too many decimal places

If the measurement uncertainty is:

$$
\pm0.02\text{ V}
$$

reporting:

$$
1.023746\text{ V}
$$

does not make scientific sense.

The precision of the reported value should be consistent with the measurement.

### Mistake 2: Confusing standard deviation and uncertainty

They are related concepts but are not automatically interchangeable.

### Mistake 3: Using percentage error without a reference value

Percentage error requires an accepted or reference value.

### Mistake 4: Giving error bars without explanation

Always state what the error bars represent.

### Mistake 5: Assuming more decimal places mean more accuracy

A computer can calculate many decimal places. That does not mean the experiment measured the quantity with that accuracy.

---

## 29.25 Key Points

- Experimental measurements contain variation.
- The mean provides a representative value.
- Range gives a simple measure of spread.
- Standard deviation describes the spread of repeated observations.
- Percentage error compares an experimental value with an accepted value.
- Uncertainty describes the limited knowledge of a measured quantity.
- Accuracy and precision are different.
- Error bars visually communicate variability or uncertainty.
- Standard deviation and standard error have different meanings.
- Calculated quantities can inherit uncertainty from measured quantities.
- Units and sensible significant figures are essential when reporting results.

---

## 29.26 Quick Practice

1. Calculate the mean of:

$$
10,\ 12,\ 11,\ 13,\ 14
$$

2. What does standard deviation describe?

3. What is the difference between accuracy and precision?

4. Write the formula for percentage error.

5. If the experimental value is 9.8 and the accepted value is 10.0, calculate the percentage error.

6. What is an error bar?

7. Why should the meaning of error bars be stated?

8. What is the difference between standard deviation and standard error?

9. Why is instrument resolution not necessarily equal to measurement uncertainty?

10. Why should excessive decimal places be avoided?

---

## 29.27 Hands-on Activity

Use repeated voltage measurements from a laboratory experiment.

For example:

```python
V = np.array([
    1.02, 1.01, 1.03,
    1.00, 1.04, 1.02,
    1.01, 1.03
])
```

Perform the following:

1. Calculate the mean.
2. Calculate the minimum and maximum.
3. Calculate the range.
4. Calculate the sample standard deviation.
5. Calculate the standard error.
6. Plot the repeated measurements.
7. Draw a horizontal line representing the mean.
8. Report the result with an appropriate number of decimal places.
9. Explain what the standard deviation tells you about the measurements.

---

## 29.28 Chapter Summary

Experimental data are never perfectly exact.

A simple workflow for analyzing measurement reliability is:

$$
\boxed{
\text{Repeated Measurements}
\rightarrow
\text{Mean}
\rightarrow
\text{Spread}
\rightarrow
\text{Uncertainty}
\rightarrow
\text{Scientific Report}
}
$$

The most important concepts are:

$$
\text{Mean}
\quad
\text{Standard Deviation}
\quad
\text{Percentage Error}
\quad
\text{Uncertainty}
$$

These concepts will be used in the final chapter of Unit II when we study **fitting, goodness of fit, and interpretation of fitted parameters**.
