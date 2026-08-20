# Chapter 10 — Statistical Operations with NumPy

## 10.1 Learning Objectives

After completing this chapter, you should be able to:

- calculate basic descriptive statistics using NumPy;
- calculate mean, median, minimum, maximum, range, variance, and standard deviation;
- understand the difference between population and sample standard deviation;
- calculate percentiles and quartiles;
- identify simple measures of spread and central tendency;
- apply statistical operations to experimental circuit and semiconductor data;
- compare repeated measurements;
- use NumPy to summarize a dataset before visualization;
- interpret numerical summaries in a scientific context.

---

10.2 Why Do We Need Statistics?

Scientific experiments rarely produce one perfect measurement.

Suppose the voltage across a device is measured five times:

$$
4.98,\quad
5.02,\quad
5.01,\quad
4.99,\quad
5.00\ \mathrm{V}.
$$

The measurements are slightly different.

This may occur because of:

- instrument resolution;
- electrical noise;
- environmental conditions;
- measurement procedure;
- variation in the experimental system.

A scientist therefore needs methods to summarize the data.

Statistics provides tools for answering questions such as:

> What is the typical value?

> How much do the measurements vary?

> What are the smallest and largest observations?

> How consistent are repeated measurements?

NumPy provides many basic statistical operations.

---

10.3 Creating a Measurement Array

Consider repeated voltage measurements:

```python
import numpy as np

V = np.array([4.98, 5.02, 5.01, 4.99, 5.00])

print(V)
```

Output:

```text
[4.98 5.02 5.01 4.99 5.  ]
```

The data is now represented as a NumPy array.

This allows statistical functions to be applied directly.

---

10.4 Mean

The arithmetic mean is:

$$
\bar{x}
=
\frac{1}{n}
\sum_{i=1}^{n}x_i.
$$

NumPy provides:

```python
np.mean()
```

For example:

```python
mean_V = np.mean(V)

print(mean_V)
```

Output:

```text
5.0
```

The mean provides a measure of the central tendency of the observations.

---

10.5 Interpreting the Mean

For:

```python
V = np.array([4.98, 5.02, 5.01, 4.99, 5.00])
```

the mean is:

$$
\bar{V}=5.00\,\mathrm{V}.
$$

This does not mean that every measurement was exactly 5 V.

It means that the arithmetic average of the observations is 5 V.

The distinction is important:

> The mean summarizes a dataset; it does not replace the individual observations.

---

10.6 Sum

NumPy provides:

```python
np.sum()
```

For example:

```python
x = np.array([1, 2, 3, 4, 5])

print(np.sum(x))
```

Output:

```text
15
```

Mathematically:

$$
\sum_{i=1}^{5}x_i
=
1+2+3+4+5
=
15.
$$

The sum is useful because the mean is calculated from the sum:

$$
\bar{x}=\frac{\sum x_i}{n}.
$$

---

10.7 Number of Observations

The number of observations can be obtained using:

```python
len(x)
```

or:

```python
x.size
```

For example:

```python
x = np.array([10, 20, 30, 40])

print(x.size)
```

Output:

```text
4
```

For a one-dimensional dataset, this gives the number of observations.

---

10.8 Minimum

The minimum value is obtained using:

```python
np.min()
```

Example:

```python
V = np.array([4.98, 5.02, 5.01, 4.99, 5.00])

print(np.min(V))
```

Output:

```text
4.98
```

The minimum is:

$$
V_{\min}=4.98\,\mathrm{V}.
$$

---

10.9 Maximum

The maximum value is obtained using:

```python
np.max()
```

Example:

```python
print(np.max(V))
```

Output:

```text
5.02
```

Therefore:

$$
V_{\max}=5.02\,\mathrm{V}.
$$

Together, minimum and maximum provide a quick view of the observed range.

---

10.10 Range

The range is:

$$
\text{Range}
=
x_{\max}-x_{\min}.
$$

Python:

```python
data_range = np.max(V) - np.min(V)

print(data_range)
```

Output:

```text
0.040000000000000036
```

For presentation:

```python
print(f"Range = {data_range:.2f} V")
```

Output:

```text
Range = 0.04 V
```

The range gives a simple measure of total spread.

---

10.11 Median

The median is the middle value after the observations are ordered.

NumPy provides:

```python
np.median()
```

For:

```python
x = np.array([1, 2, 3, 4, 5])
```

we have:

```python
print(np.median(x))
```

Output:

```text
3.0
```

The median is less affected by extreme values than the mean.

---

10.12 Mean versus Median

Consider:

```python
x = np.array([10, 11, 12, 13, 100])
```

Calculate:

```python
print(np.mean(x))
print(np.median(x))
```

The mean is strongly influenced by the value 100.

The median remains close to the central observations.

This illustrates an important idea:

> The mean and median describe central tendency in different ways.

Neither is universally "better"; the appropriate measure depends on the data and scientific question.

---

10.13 Percentiles

A percentile tells us the value below which a specified percentage of observations falls.

NumPy provides:

```python
np.percentile()
```

For example:

```python
x = np.array([10, 20, 30, 40, 50])

print(np.percentile(x, 50))
```

Output:

```text
30.0
```

The 50th percentile corresponds to the median.

We can calculate the 25th percentile:

```python
print(np.percentile(x, 25))
```

and the 75th percentile:

```python
print(np.percentile(x, 75))
```

These values help describe the distribution of observations.

---

10.14 Quartiles

The quartiles divide ordered data into four parts.

The common definitions are:

$$
Q_1=\text{25th percentile},
$$

$$
Q_2=\text{50th percentile},
$$

$$
Q_3=\text{75th percentile}.
$$

We can calculate them using:

```python
Q1 = np.percentile(x, 25)
Q2 = np.percentile(x, 50)
Q3 = np.percentile(x, 75)
```

The second quartile is the median:

$$
Q_2=\text{median}.
$$

---

10.15 Interquartile Range

The interquartile range is:

$$
IQR=Q_3-Q_1.
$$

Python:

```python
Q1 = np.percentile(x, 25)
Q3 = np.percentile(x, 75)

IQR = Q3 - Q1

print(IQR)
```

The IQR measures the spread of the middle 50% of the observations.

It is less affected by extreme observations than the full range.

---

10.16 Variance

Variance measures the average squared deviation from the mean.

For a population:

$$
\sigma^2
=
\frac{1}{N}
\sum_{i=1}^{N}
(x_i-\mu)^2.
$$

For a sample:

$$
s^2
=
\frac{1}{n-1}
\sum_{i=1}^{n}
(x_i-\bar{x})^2.
$$

NumPy provides:

```python
np.var()
```

By default, NumPy uses:

$$
\frac{1}{N}
$$

for the variance calculation.

---

10.17 Population Variance in NumPy

Example:

```python
x = np.array([1, 2, 3, 4, 5])

variance = np.var(x)

print(variance)
```

Output:

```text
2.0
```

The mean is:

$$
\bar{x}=3.
$$

The squared deviations are:

$$
(1-3)^2=4,
$$

$$
(2-3)^2=1,
$$

$$
(3-3)^2=0,
$$

$$
(4-3)^2=1,
$$

$$
(5-3)^2=4.
$$

Therefore:

$$
\sigma^2
=
\frac{4+1+0+1+4}{5}
=
2.
$$

---

10.18 Sample Variance

When the data represents a sample from a larger population, the sample variance is commonly calculated using:

$$
s^2
=
\frac{1}{n-1}
\sum_{i=1}^{n}
(x_i-\bar{x})^2.
$$

NumPy allows this using:

```python
np.var(x, ddof=1)
```

For example:

```python
x = np.array([1, 2, 3, 4, 5])

sample_variance = np.var(x, ddof=1)

print(sample_variance)
```

Output:

```text
2.5
```

The parameter:

```python
ddof=1
```

changes the denominator from:

$$
n
$$

to:

$$
n-1.
$$

---

10.19 Standard Deviation

Variance is measured in squared units.

For example, if voltage is measured in volts, voltage variance has units:

$$
\mathrm{V}^2.
$$

The square root of variance returns the original units.

This is the **standard deviation**.

For a population:

$$
\sigma
=
\sqrt{
\frac{1}{N}
\sum_{i=1}^{N}(x_i-\mu)^2
}.
$$

NumPy provides:

```python
np.std()
```

---

10.20 Population Standard Deviation

Example:

```python
x = np.array([1, 2, 3, 4, 5])

std = np.std(x)

print(std)
```

Output:

```text
1.4142135623730951
```

This is:

$$
\sqrt{2}\approx1.414.
$$

The standard deviation is expressed in the same units as the original measurements.

---

10.21 Sample Standard Deviation

For a sample:

```python
x = np.array([1, 2, 3, 4, 5])

std_sample = np.std(x, ddof=1)

print(std_sample)
```

Output:

```text
1.5811388300841898
```

The distinction between population and sample standard deviation is important in statistical analysis.

A useful reminder is:

```text
np.std(x)
       ↓
denominator n

np.std(x, ddof=1)
       ↓
denominator n - 1
```

---

10.22 Why Standard Deviation Matters Experimentally

Suppose two experiments both have a mean voltage of:

$$
5.00\,\mathrm{V}.
$$

Experiment A:

```text
4.99, 5.00, 5.01, 5.00, 5.00
```

Experiment B:

```text
4.80, 5.20, 4.90, 5.10, 5.00
```

Both may have approximately the same mean.

But Experiment B has much greater variation.

The standard deviation helps quantify this difference.

This illustrates:

> A mean alone is not enough to describe experimental data.

A measure of spread is also required.

---

10.23 Calculating Several Statistics Together

Consider:

```python
V = np.array([4.98, 5.02, 5.01, 4.99, 5.00])
```

We can calculate:

```python
mean_V = np.mean(V)
median_V = np.median(V)
min_V = np.min(V)
max_V = np.max(V)
std_V = np.std(V)

print(f"Mean   = {mean_V:.3f} V")
print(f"Median = {median_V:.3f} V")
print(f"Minimum = {min_V:.3f} V")
print(f"Maximum = {max_V:.3f} V")
print(f"Std. Dev. = {std_V:.4f} V")
```

This creates a compact numerical summary of the experiment.

---

10.24 A Simple Statistical Summary

For an experimental dataset, a useful basic summary is:

$$
\boxed{
n,\quad
\bar{x},\quad
\text{median},\quad
x_{\min},\quad
x_{\max},\quad
\sigma
}
$$

where:

- \(n\) = number of observations;
- \(\bar{x}\) = mean;
- median = middle value;
- \(x_{\min}\) = minimum;
- \(x_{\max}\) = maximum;
- \(\sigma\) = standard deviation.

These statistics do not replace the raw measurements.

They summarize them.

---

10.25 Statistical Operations on Semiconductor Measurements

Suppose repeated measurements of a diode forward voltage are:

```python
Vf = np.array([
    0.68,
    0.71,
    0.70,
    0.69,
    0.72,
    0.70
])
```

We can calculate:

```python
mean_Vf = np.mean(Vf)
std_Vf = np.std(Vf, ddof=1)

print(f"Mean forward voltage = {mean_Vf:.3f} V")
print(f"Sample standard deviation = {std_Vf:.4f} V")
```

The result gives a compact description of the repeated measurements.

The units remain volts.

---

10.26 Current Measurements

Suppose current measurements are:

```python
I = np.array([
    1.02,
    0.98,
    1.01,
    1.05,
    0.99
])
```

If these values are measured in mA, we can calculate:

```python
mean_I = np.mean(I)
std_I = np.std(I, ddof=1)

print(f"Mean current = {mean_I:.3f} mA")
print(f"Sample standard deviation = {std_I:.3f} mA")
```

The units of the statistical quantities follow the units of the original data.

---

10.27 Statistical Operations and Units

Suppose:

```python
T = np.array([298, 300, 302, 301, 299])
```

where temperature is measured in kelvin.

Then:

```python
np.mean(T)
```

has units:

$$
\mathrm{K}.
$$

But:

```python
np.var(T)
```

has units:

$$
\mathrm{K}^2.
$$

And:

```python
np.std(T)
```

has units:

$$
\mathrm{K}.
$$

This is important when interpreting numerical results scientifically.

---

10.28 Axis: A First Look

NumPy arrays can have more than one dimension.

Consider:

```python
data = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
```

We can calculate the mean of all values:

```python
print(np.mean(data))
```

Output:

```text
3.5
```

But sometimes we want the mean of each column.

We can specify:

```python
print(np.mean(data, axis=0))
```

Output:

```text
[2.5 3.5 4.5]
```

This calculates:

$$
\begin{aligned}
\bar{x}_1 &= \frac{1+4}{2}=2.5,\\
\bar{x}_2 &= \frac{2+5}{2}=3.5,\\
\bar{x}_3 &= \frac{3+6}{2}=4.5.
\end{aligned}
$$

---

10.29 Mean Along Rows

We can calculate the mean of each row using:

```python
print(np.mean(data, axis=1))
```

Output:

```text
[2. 5.]
```

The first row mean is:

$$
\frac{1+2+3}{3}=2,
$$

and the second row mean is:

$$
\frac{4+5+6}{3}=5.
$$

The `axis` concept becomes important when working with experimental data organized as tables.

---

10.30 A Measurement Table

Consider:

```python
data = np.array([
    [0.1, 0.001, 300],
    [0.2, 0.002, 300],
    [0.3, 0.004, 301],
    [0.4, 0.008, 299]
])
```

The columns represent:

```text
Voltage | Current | Temperature
```

We can calculate the mean of each column:

```python
mean_values = np.mean(data, axis=0)

print(mean_values)
```

The result contains the mean voltage, mean current, and mean temperature.

This is a useful introduction to statistical analysis of tabular numerical data.

---

10.31 Standard Deviation Along an Axis

The same `axis` concept applies to standard deviation.

```python
std_values = np.std(data, axis=0)

print(std_values)
```

This calculates the standard deviation of each column.

For sample standard deviation:

```python
std_values = np.std(data, axis=0, ddof=1)
```

The meaning of `ddof=1` remains the same.

---

10.32 `keepdims`

NumPy provides the optional argument:

```python
keepdims=True
```

For example:

```python
data = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

mean_values = np.mean(data, axis=0, keepdims=True)

print(mean_values)
```

Output:

```text
[[2.5 3.5 4.5]]
```

The dimensions are preserved.

This becomes useful when the statistical result will later be combined with the original array.

The detailed use of broadcasting with dimensions will be covered as needed.

---

10.33 Common Beginner Mistakes

### Mistake 1 — Reporting only the mean

A mean does not show how much the measurements vary.

Include an appropriate measure of spread when needed.

### Mistake 2 — Confusing variance and standard deviation

Variance has squared units.

Standard deviation has the original units.

### Mistake 3 — Ignoring `ddof`

For sample standard deviation, use:

```python
np.std(x, ddof=1)
```

when that statistical convention is appropriate.

### Mistake 4 — Confusing median and mean

The median is not generally the same as the mean.

### Mistake 5 — Ignoring units

Statistical calculations do not remove physical units.

### Mistake 6 — Using `axis` incorrectly

For a two-dimensional array:

```python
axis=0
```

operates down the rows and produces column-wise results.

```python
axis=1
```

operates across columns and produces row-wise results.

---

10.34 Worked Example: Repeated Voltage Measurements

A resistor is connected to a power supply and the voltage is measured five times:

```python
import numpy as np

V = np.array([4.98, 5.02, 5.01, 4.99, 5.00])

n = V.size
mean_V = np.mean(V)
median_V = np.median(V)
min_V = np.min(V)
max_V = np.max(V)
std_V = np.std(V, ddof=1)

print(f"Number of measurements = {n}")
print(f"Mean voltage = {mean_V:.3f} V")
print(f"Median voltage = {median_V:.3f} V")
print(f"Minimum voltage = {min_V:.3f} V")
print(f"Maximum voltage = {max_V:.3f} V")
print(f"Sample standard deviation = {std_V:.4f} V")
```

This provides a concise statistical description of the measurement series.

---

10.35 Worked Example: Semiconductor Temperature Measurements

Suppose the temperature of a device is recorded during an experiment:

```python
T = np.array([298.2, 299.1, 300.0, 299.5, 300.3, 298.9])
```

Calculate:

```python
mean_T = np.mean(T)
std_T = np.std(T, ddof=1)

print(f"Mean temperature = {mean_T:.2f} K")
print(f"Sample standard deviation = {std_T:.2f} K")
```

Interpretation:

- the mean represents the typical observed temperature;
- the standard deviation indicates the variation around the mean.

The statistics should always be interpreted in the context of the experiment.

---

10.36 Worked Example: Compare Two Experiments

Experiment A:

```python
A = np.array([4.99, 5.00, 5.01, 5.00, 5.00])
```

Experiment B:

```python
B = np.array([4.80, 5.20, 4.90, 5.10, 5.00])
```

Calculate:

```python
print("Experiment A")
print("Mean:", np.mean(A))
print("Std:", np.std(A, ddof=1))

print()

print("Experiment B")
print("Mean:", np.mean(B))
print("Std:", np.std(B, ddof=1))
```

Both experiments may have a similar mean.

However, Experiment B has a larger standard deviation.

This indicates greater measurement variation.

The example demonstrates why both central tendency and spread should be considered.

---

10.37 Exercises

### Exercise 1 — Mean

Create:

```python
x = np.array([10, 12, 11, 13, 14])
```

Calculate the mean.

---

### Exercise 2 — Median

Using the same dataset, calculate the median.

---

### Exercise 3 — Range

Calculate:

$$
x_{\max}-x_{\min}.
$$

---

### Exercise 4 — Standard Deviation

Calculate both:

```python
np.std(x)
```

and:

```python
np.std(x, ddof=1)
```

Explain the difference.

---

### Exercise 5 — Quartiles

For:

```python
x = np.array([10, 12, 11, 13, 14, 15, 16, 18])
```

calculate:

$$
Q_1,\quad Q_2,\quad Q_3.
$$

Then calculate:

$$
IQR=Q_3-Q_1.
$$

---

### Exercise 6 — Semiconductor Voltage

Create repeated forward-voltage measurements:

```text
0.68, 0.71, 0.70, 0.69, 0.72, 0.70 V
```

Calculate:

- mean;
- median;
- minimum;
- maximum;
- sample standard deviation.

---

### Exercise 7 — Current Measurements

Create an array of five current measurements in mA.

Calculate the mean and sample standard deviation.

Report the results with appropriate units.

---

### Exercise 8 — Two-Dimensional Data

Create:

```python
data = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])
```

Calculate:

1. the overall mean;
2. the mean of each column;
3. the mean of each row.

---

10.38 Think and Apply

Two semiconductor devices have the following repeated measurements of forward voltage.

Device A:

```text
0.699, 0.700, 0.701, 0.700, 0.700 V
```

Device B:

```text
0.68, 0.72, 0.69, 0.71, 0.70 V
```

Both devices may have a similar average.

Which device shows more consistent measurements?

What statistic would you use to support your answer?

---

10.39 Chapter Summary

In this chapter, we learned that:

1. Statistics helps summarize experimental measurements.
2. `np.mean()` calculates the arithmetic mean.
3. `np.median()` calculates the median.
4. `np.min()` and `np.max()` identify extreme values.
5. The range measures the total spread.
6. `np.percentile()` can calculate percentiles and quartiles.
7. The interquartile range is:
   $$
   IQR=Q_3-Q_1.
   $$
8. `np.var()` calculates variance.
9. `np.std()` calculates standard deviation.
10. `ddof=1` is commonly used for sample variance and sample standard deviation.
11. Standard deviation has the same units as the original measurement.
12. The `axis` argument allows statistical operations to be performed across rows or columns.
13. Mean and spread should usually be considered together.
14. NumPy provides a convenient foundation for statistical analysis of scientific measurements.
15. Statistical summaries are useful before visualization and further modelling.

---

10.40 Key Takeaway

> **A scientific measurement is rarely understood by a single number. NumPy provides the basic statistical tools needed to summarize central tendency, variation, and the overall behavior of experimental data.**

The next chapter introduces **Matplotlib**, where these numerical arrays and statistical summaries become visual scientific information through plots and graphs.
