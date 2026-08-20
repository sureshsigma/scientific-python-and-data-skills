# Chapter 12 — NumPy with Scientific and Device Data

## 12.1 Learning Objectives

After completing this chapter, you should be able to:

- represent scientific measurements using NumPy arrays;
- organize voltage, current, temperature, and other device variables;
- distinguish between measured data and calculated data;
- perform calculations on experimental arrays;
- calculate resistance and power from circuit measurements;
- work with repeated measurements;
- organize basic circuit and semiconductor device datasets;
- inspect numerical data before visualization;
- prepare arrays for plotting and further analysis;
- understand the role of NumPy as a bridge between experimental data and scientific visualization.

---

12.2 From Numerical Arrays to Scientific Data

In the previous chapters, we learned how to:

- create NumPy arrays;
- generate numerical values;
- perform mathematical calculations;
- use vectorized operations;
- generate scientific input grids.

The next step is to use these skills with data that represents real or simulated scientific measurements.

For example, an electronics experiment may produce:

```text
Voltage
Current
Temperature
```

A semiconductor experiment may produce:

```text
Applied voltage
Device current
Temperature
```

These measurements can be represented using NumPy arrays.

The basic workflow is:

```text
Experiment / Instrument
        ↓
Measurements
        ↓
NumPy arrays
        ↓
Cleaning / checking
        ↓
Numerical calculations
        ↓
Visualization
        ↓
Scientific interpretation
```

---

12.3 Measurement Data versus Generated Data

It is important to distinguish between two types of numerical arrays.

### Measured data

Measured data comes from an experiment, sensor, or instrument.

For example:

```python
V_measured = np.array([
    0.10,
    0.21,
    0.31,
    0.39,
    0.50
])
```

These values represent observations.

### Generated data

Generated data is produced computationally.

For example:

```python
V_model = np.linspace(0.1, 0.5, 100)
```

These values form a numerical grid.

They are not measurements.

This distinction should be maintained throughout scientific analysis.

---

12.4 Representing Voltage Measurements

Suppose an experiment records five voltage measurements:

```python
import numpy as np

V = np.array([
    1.02,
    1.98,
    3.01,
    4.02,
    4.99
])
```

The array represents:

$$
V=
\begin{bmatrix}
1.02\\
1.98\\
3.01\\
4.02\\
4.99
\end{bmatrix}
\mathrm{V}.
$$

We can inspect:

```python
print(V)
print(V.size)
print(V.dtype)
```

This provides basic information about the dataset.

---

12.5 Representing Current Measurements

Suppose the corresponding current measurements are:

```python
I = np.array([
    0.010,
    0.019,
    0.030,
    0.041,
    0.050
])
```

The two arrays are paired.

That means:

```text
V[0] corresponds to I[0]
V[1] corresponds to I[1]
V[2] corresponds to I[2]
...
```

Therefore, the first observation is:

$$
(V_1,I_1)=(1.02\,\mathrm{V},0.010\,\mathrm{A}).
$$

This pairing is fundamental in experimental data analysis.

---

12.6 Why Equal Array Lengths Matter

Suppose:

```python
V = np.array([1, 2, 3, 4, 5])
I = np.array([0.01, 0.02, 0.03])
```

The arrays contain different numbers of observations.

This creates a problem because there is no current value corresponding to the fourth and fifth voltage measurements.

Before performing paired calculations or plotting one variable against another, verify that the arrays have compatible lengths.

For example:

```python
print(V.size)
print(I.size)
```

If the sizes are different, investigate the data before continuing.

---

12.7 Basic Data Inspection

Before calculating anything, inspect the data.

For an array `x`, useful checks include:

```python
print(x)
print(x.size)
print(x.shape)
print(x.dtype)
```

We can also calculate:

```python
print(np.min(x))
print(np.max(x))
print(np.mean(x))
```

These simple checks can reveal obvious problems.

A useful scientific habit is:

> **Inspect first, calculate second.**

---

12.8 Temperature Arrays

Temperature is another common experimental variable.

For example:

```python
T = np.array([
    298.1,
    299.0,
    300.2,
    301.1,
    299.8
])
```

If the values are in kelvin:

$$
T\;[\mathrm{K}].
$$

The array can be used directly in numerical calculations.

For example, calculate the mean temperature:

```python
mean_T = np.mean(T)

print(mean_T)
```

---

12.9 Temperature Conversion

Suppose measurements are recorded in Celsius:

```python
T_C = np.array([25, 27, 30, 32, 35])
```

Convert to kelvin:

$$
T_K=T_C+273.15.
$$

Python:

```python
T_K = T_C + 273.15

print(T_K)
```

The conversion is applied to every measurement.

The original Celsius array can be retained so that the raw data is not lost.

---

12.10 Why Unit Consistency Matters

Suppose voltage is represented in volts:

```python
V = np.array([0.1, 0.2, 0.3])
```

and current is represented in milliamperes:

```python
I_mA = np.array([1, 2, 3])
```

Before calculating resistance:

$$
R=\frac{V}{I},
$$

we must ensure that the units are consistent.

Convert current to amperes:

$$
I_A=I_{\mathrm{mA}}\times10^{-3}.
$$

Python:

```python
I_A = I_mA * 1e-3
```

Then:

```python
R = V / I_A
```

The result is in ohms.

This is a critical scientific computing habit:

> Numerical correctness does not guarantee physical correctness.

Units must also be correct.

---

12.11 Resistance from Voltage and Current

Ohm's law gives:

$$
R=\frac{V}{I}.
$$

Suppose:

```python
V = np.array([1, 2, 3, 4, 5])
I = np.array([0.01, 0.02, 0.03, 0.04, 0.05])
```

Calculate:

```python
R = V / I

print(R)
```

Output:

```text
[100. 100. 100. 100. 100.]
```

The resistance is approximately constant.

This is consistent with an ideal resistor model.

---

12.12 Resistance from Experimental Data

Now consider:

```python
V = np.array([
    1.0,
    2.0,
    3.0,
    4.0,
    5.0
])

I = np.array([
    0.010,
    0.019,
    0.031,
    0.039,
    0.051
])
```

Calculate:

```python
R = V / I

print(R)
```

The resistance values are no longer identical.

This may occur because of:

- measurement variation;
- instrument uncertainty;
- noise;
- temperature effects;
- non-ideal device behavior.

The calculation provides a new derived variable:

$$
R_i=\frac{V_i}{I_i}.
$$

---

12.13 Calculated Variables

Scientific datasets often contain both measured and calculated variables.

For example:

```text
Measured:
V
I
T

Calculated:
R = V / I
P = V I
```

This can be represented as:

```text
Voltage ───────┐
               ├──→ Resistance
Current ───────┘

Voltage ───────┐
               ├──→ Power
Current ───────┘
```

The important idea is that calculated variables should be clearly distinguished from measured variables.

---

12.14 Power from Experimental Measurements

Electrical power is:

$$
P=VI.
$$

Suppose:

```python
V = np.array([1, 2, 3, 4, 5])
I = np.array([0.010, 0.020, 0.030, 0.040, 0.050])
```

Calculate:

```python
P = V * I

print(P)
```

Output:

```text
[0.01 0.04 0.09 0.16 0.25]
```

The unit is watts when voltage is in volts and current is in amperes.

---

12.15 Creating a Basic Circuit Dataset

We can organize the variables:

```python
import numpy as np

V = np.array([1, 2, 3, 4, 5])
I = np.array([0.010, 0.020, 0.030, 0.040, 0.050])

R = V / I
P = V * I
```

The resulting dataset conceptually contains:

| Voltage (V) | Current (A) | Resistance (ohm) | Power (W) |
| ---: | ---: | ---: | ---: |
| 1 | 0.010 | 100 | 0.01 |
| 2 | 0.020 | 100 | 0.04 |
| 3 | 0.030 | 100 | 0.09 |
| 4 | 0.040 | 100 | 0.16 |
| 5 | 0.050 | 100 | 0.25 |

This is a simple example of derived scientific data.

---

12.16 Repeated Measurements

Experimental measurements are often repeated.

Suppose voltage is measured five times:

```python
V = np.array([
    4.98,
    5.02,
    5.01,
    4.99,
    5.00
])
```

We can calculate:

```python
mean_V = np.mean(V)
std_V = np.std(V, ddof=1)

print(mean_V)
print(std_V)
```

The mean describes the central value.

The sample standard deviation describes variation among the repeated measurements.

---

12.17 Repeated Measurements at Multiple Conditions

Sometimes measurements are repeated at several input conditions.

For example, voltage may be measured three times at each setting.

A two-dimensional array can represent this:

```python
V = np.array([
    [1.01, 1.00, 0.99],
    [2.02, 2.01, 1.98],
    [3.01, 3.00, 3.02]
])
```

Each row represents one nominal voltage condition.

Each column represents a repeated measurement.

The shape is:

```python
print(V.shape)
```

Output:

```text
(3, 3)
```

---

12.18 Mean of Repeated Measurements

Calculate the mean of each row:

```python
mean_V = np.mean(V, axis=1)

print(mean_V)
```

The result gives one representative voltage for each condition.

For example:

$$
\bar{V}_j
=
\frac{1}{m}
\sum_{i=1}^{m}V_{ji},
$$

where \(m\) is the number of repeated measurements.

This is a practical example of using the `axis` argument.

---

12.19 Standard Deviation of Repeated Measurements

Similarly:

```python
std_V = np.std(V, axis=1, ddof=1)

print(std_V)
```

This calculates the sample standard deviation for each condition.

Now each experimental condition can be summarized using:

```text
Mean ± standard deviation
```

For example:

```text
1.00 ± 0.01 V
```

The exact reporting convention depends on the experiment and uncertainty methodology.

---

12.20 Introduction to Semiconductor Device Data

Semiconductor device experiments commonly involve relationships between variables such as:

- applied voltage;
- current;
- temperature;
- illumination;
- time;
- device geometry.

Examples include:

### Diode

$$
I=f(V).
$$

### LED

$$
I=f(V).
$$

### Photodiode

$$
I=f(\text{illumination}).
$$

### Solar cell

$$
I=f(V,\text{illumination}).
$$

### Temperature-dependent device

$$
I=f(V,T).
$$

The exact physical models can be complex.

At this stage, our objective is to learn how to represent and prepare the numerical data.

---

12.21 Diode I-V Data

Suppose a diode experiment produces:

```python
V = np.array([
    0.10,
    0.20,
    0.30,
    0.40,
    0.50,
    0.60,
    0.70
])

I = np.array([
    0.001,
    0.002,
    0.004,
    0.008,
    0.015,
    0.030,
    0.060
])
```

The arrays represent paired observations:

$$
(V_i,I_i).
$$

The scientific question may be:

> How does current change as voltage changes?

This dataset is ready for visualization.

---

12.22 Inspecting Diode Data

Before plotting:

```python
print(V.size)
print(I.size)

print(np.min(V))
print(np.max(V))

print(np.min(I))
print(np.max(I))
```

We can also inspect:

```python
print(V.dtype)
print(I.dtype)
```

This basic inspection can identify obvious problems before visualization.

---

12.23 Logarithmic Transformation of Current

Semiconductor current can span several orders of magnitude.

Suppose:

```python
I = np.array([
    1e-12,
    1e-10,
    1e-8,
    1e-6,
    1e-4
])
```

Calculate:

```python
log_I = np.log10(I)

print(log_I)
```

Output:

```text
[-12. -10.  -8.  -6.  -4.]
```

The logarithmic representation can make large numerical ranges easier to analyze.

This does not change the original current measurements.

It creates a transformed representation.

---

12.24 Creating a Temperature-Dependent Dataset

Suppose a device is measured at several temperatures.

```python
T = np.array([300, 310, 320, 330, 340])

R = np.array([
    100.0,
    104.1,
    108.3,
    112.4,
    116.5
])
```

These arrays represent:

$$
(T_i,R_i).
$$

The scientific question may be:

> How does resistance change with temperature?

The data can later be plotted and analyzed.

---

12.25 Device Data with Multiple Variables

Suppose an experiment records:

```python
V = np.array([0.1, 0.2, 0.3, 0.4])
I = np.array([0.001, 0.002, 0.004, 0.008])
T = np.array([300, 300, 301, 301])
```

Each index represents one observation.

For example:

```python
V[2]
I[2]
T[2]
```

together represent the third observation.

Conceptually:

$$
\text{Observation}_i
=
(V_i,I_i,T_i).
$$

This indexing relationship is essential when combining multiple experimental variables.

---

12.26 Keeping Paired Data Together

Suppose:

```python
V = np.array([0.1, 0.2, 0.3])
I = np.array([0.001, 0.002, 0.004])
```

If we reorder voltage without reordering current:

```python
V = np.array([0.3, 0.1, 0.2])
```

but leave `I` unchanged, the observations become incorrectly paired.

The original relationship:

```text
0.1 → 0.001
0.2 → 0.002
0.3 → 0.004
```

has been broken.

Therefore:

> When observations contain paired or related variables, their row/index correspondence must be preserved.

---

12.27 Sorting Data Carefully

Suppose voltage values are not in increasing order.

We can obtain sorting indices:

```python
order = np.argsort(V)
```

Then apply the same ordering to related arrays:

```python
V_sorted = V[order]
I_sorted = I[order]
```

Now the voltage and current observations remain paired.

This is an important data preparation technique.

---

12.28 Checking for Missing Values

Experimental data may contain missing numerical values.

For example:

```python
x = np.array([1.0, 2.0, np.nan, 4.0])
```

Here:

```text
np.nan
```

represents a missing or undefined numerical value.

We can identify it using:

```python
print(np.isnan(x))
```

Output:

```text
[False False  True False]
```

We can count missing values:

```python
print(np.sum(np.isnan(x)))
```

Output:

```text
1
```

Handling missing data properly is an important part of scientific data preparation.

---

12.29 Checking for Infinite Values

Numerical calculations can sometimes produce:

```text
inf
```

or:

```text
-inf
```

For example, division by zero can lead to an infinite result.

NumPy provides:

```python
np.isinf()
```

Example:

```python
x = np.array([1, 2, np.inf, 4])

print(np.isinf(x))
```

Output:

```text
[False False  True False]
```

We can check whether any infinite values exist:

```python
print(np.any(np.isinf(x)))
```

---

12.30 Checking for Finite Values

NumPy provides:

```python
np.isfinite()
```

For example:

```python
x = np.array([1, 2, np.nan, np.inf])

print(np.isfinite(x))
```

Output:

```text
[ True  True False False]
```

This provides a simple way to identify valid finite numerical observations.

---

12.31 Basic Data Validation

Before visualization or modelling, a basic validation routine might include:

```python
print("Number of observations:", V.size)
print("Minimum:", np.min(V))
print("Maximum:", np.max(V))
print("Missing:", np.sum(np.isnan(V)))
print("Finite:", np.all(np.isfinite(V)))
```

The exact checks will depend on the experiment.

The principle is:

> **Do not assume that numerical data is correct simply because Python can store it.**

---

12.32 Preparing Arrays for Visualization

Suppose:

```python
V = np.array([0.1, 0.2, 0.3, 0.4, 0.5])
I = np.array([0.001, 0.002, 0.004, 0.008, 0.015])
```

Before plotting, check:

```python
print(V.size)
print(I.size)

print(np.all(np.isfinite(V)))
print(np.all(np.isfinite(I)))
```

If the arrays are compatible and valid, they can be passed to Matplotlib.

For example:

```python
plt.scatter(V, I)
```

The actual plotting procedure will be developed in the visualization chapters.

---

12.33 Creating a Device Data Dictionary

For a small experiment, a Python dictionary can organize related arrays:

```python
data = {
    "voltage": np.array([0.1, 0.2, 0.3, 0.4]),
    "current": np.array([0.001, 0.002, 0.004, 0.008]),
    "temperature": np.array([300, 300, 301, 301])
}
```

We can access:

```python
data["voltage"]
```

or:

```python
data["current"]
```

This provides a simple structure for keeping related scientific variables together.

---

12.34 Adding Calculated Variables

We can add derived quantities:

```python
data["resistance"] = (
    data["voltage"] / data["current"]
)
```

and:

```python
data["power"] = (
    data["voltage"] * data["current"]
)
```

The dictionary now contains:

```text
voltage
current
temperature
resistance
power
```

This is a simple example of building a scientific dataset programmatically.

---

12.35 Preparing Data for Later File Export

Once the data has been organized, it can later be saved to a CSV or other file format.

For example, the conceptual table is:

```text
Voltage | Current | Temperature | Resistance | Power
```

This is an important transition:

```text
Raw measurements
       ↓
NumPy arrays
       ↓
Derived variables
       ↓
Organized dataset
       ↓
File / visualization / analysis
```

File handling was introduced earlier; NumPy now provides the numerical processing layer.

---

12.36 Worked Example: Basic Circuit Dataset

Suppose an experiment records voltage and current:

```python
import numpy as np

V = np.array([
    1.0,
    2.0,
    3.0,
    4.0,
    5.0
])

I = np.array([
    0.010,
    0.019,
    0.031,
    0.039,
    0.051
])
```

Check the data:

```python
print("Number of voltage observations:", V.size)
print("Number of current observations:", I.size)
```

Check finite values:

```python
print("Voltage valid:", np.all(np.isfinite(V)))
print("Current valid:", np.all(np.isfinite(I)))
```

Calculate resistance:

```python
R = V / I
```

Calculate power:

```python
P = V * I
```

Display:

```python
print("Resistance:", R)
print("Power:", P)
```

The experiment now has measured and derived variables.

---

12.37 Worked Example: Repeated Device Measurements

Suppose forward voltage is measured six times:

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

Calculate:

```python
mean_Vf = np.mean(Vf)
std_Vf = np.std(Vf, ddof=1)
```

Display:

```python
print(f"Mean forward voltage = {mean_Vf:.3f} V")
print(f"Sample standard deviation = {std_Vf:.4f} V")
```

This converts a collection of repeated measurements into a basic statistical summary.

---

12.38 Worked Example: Device Data with Temperature

Suppose:

```python
T = np.array([300, 310, 320, 330, 340])

R = np.array([
    100.0,
    104.0,
    108.0,
    112.0,
    116.0
])
```

Check:

```python
print("Temperature range:", np.min(T), "to", np.max(T))
print("Resistance range:", np.min(R), "to", np.max(R))
```

The arrays are now ready for a plot of:

$$
R \text{ versus } T.
$$

---

12.39 Worked Example: Preparing Diode Data

Suppose:

```python
V = np.array([
    0.10,
    0.20,
    0.30,
    0.40,
    0.50,
    0.60,
    0.70
])

I = np.array([
    1e-6,
    2e-6,
    5e-6,
    1e-5,
    3e-5,
    1e-4,
    5e-4
])
```

First validate:

```python
print("Same length:", V.size == I.size)
print("Voltage valid:", np.all(np.isfinite(V)))
print("Current valid:", np.all(np.isfinite(I)))
```

Then create a logarithmic representation:

```python
log_I = np.log10(I)
```

Now we have:

```text
V
I
log_I
```

The raw current remains available, while the transformed current can be used for alternative analysis or visualization.

---

12.40 Scientific Interpretation Begins After Preparation

NumPy can calculate:

```python
R = V / I
```

or:

```python
log_I = np.log10(I)
```

But NumPy does not automatically decide what the result means physically.

The scientist must ask:

- Is the result physically reasonable?
- Are the units correct?
- Does the trend agree with the expected behavior?
- Are there unusual observations?
- Is the model appropriate?
- Could an instrument or experimental condition explain the behavior?

This distinction is fundamental:

> **Computation produces numbers; scientific reasoning interprets them.**

---

12.41 Common Beginner Mistakes

### Mistake 1 — Mixing units

Do not divide volts by milliamperes and label the result as ohms without accounting for the factor of \(10^{-3}\).

### Mistake 2 — Breaking paired observations

If voltage and current correspond to the same measurements, they must remain aligned.

### Mistake 3 — Modifying raw data unnecessarily

Keep the original measurements unchanged.

Create new arrays for transformed or cleaned data.

### Mistake 4 — Ignoring missing values

Check for:

```python
np.isnan()
```

before statistical calculations or visualization.

### Mistake 5 — Ignoring infinite values

Check for:

```python
np.isinf()
```

especially after division or other numerical operations.

### Mistake 6 — Treating a generated grid as measured data

A `linspace()` array is a computational grid, not an experimental dataset.

### Mistake 7 — Calculating before inspecting

Always perform basic checks before applying scientific equations.

---

12.42 A Practical Data Preparation Checklist

Before analyzing a scientific dataset, ask:

### Structure

- How many observations are present?
- What variables are available?
- Are related arrays the same length?

### Units

- What are the units?
- Are the units consistent?

### Validity

- Are there missing values?
- Are there infinite values?
- Are there obviously impossible values?

### Relationships

- Which variables are paired?
- Does each index represent the same observation?

### Transformation

- Are unit conversions required?
- Are calculated variables needed?
- Is a logarithmic transformation appropriate?

### Visualization

- Which variable will be on the x-axis?
- Which variable will be on the y-axis?
- Is the dataset ready for plotting?

This checklist forms a simple scientific data-preparation habit.

---

12.43 Exercises

### Exercise 1 — Voltage Data

Create a NumPy array containing five voltage measurements.

Calculate:

- number of observations;
- minimum;
- maximum;
- mean.

---

### Exercise 2 — Current Data

Create a corresponding current array.

Verify that the voltage and current arrays have the same length.

---

### Exercise 3 — Resistance

Calculate:

$$
R=\frac{V}{I}.
$$

Check whether the resistance is approximately constant.

---

### Exercise 4 — Power

Calculate:

$$
P=VI.
$$

Report the result in watts.

---

### Exercise 5 — Temperature

Create an array of temperature measurements in Celsius.

Convert it to Kelvin.

Keep both arrays.

---

### Exercise 6 — Repeated Measurements

Create six repeated measurements of a device voltage.

Calculate:

- mean;
- median;
- sample standard deviation;
- minimum;
- maximum.

---

### Exercise 7 — Missing Data

Create:

```python
x = np.array([1.0, 2.0, np.nan, 4.0, 5.0])
```

Identify the missing value.

---

### Exercise 8 — Infinite Data

Create:

```python
x = np.array([1.0, 2.0, np.inf, 4.0])
```

Identify the infinite value.

---

### Exercise 9 — Diode Data

Create voltage and current arrays representing a hypothetical diode I-V experiment.

Verify:

- equal lengths;
- finite values;
- minimum and maximum current.

---

### Exercise 10 — Data Preparation

Create a small device dataset containing:

```text
Voltage
Current
Temperature
```

Calculate:

```text
Resistance
Power
```

Then organize all variables in a dictionary.

---

12.44 Think and Apply

Suppose an experimental dataset contains:

```text
Voltage:    V
Current:    I
Temperature: T
```

A student immediately calculates:

$$
R=\frac{V}{I}.
$$

What should the student check first?

Consider:

1. Are \(V\) and \(I\) paired correctly?
2. Are the arrays the same length?
3. Are the units consistent?
4. Are there missing values?
5. Are there zero-current observations?
6. Are the numerical values physically reasonable?

Scientific data analysis should begin with these questions rather than with calculations alone.

---

12.45 Chapter Summary

In this chapter, we learned that:

1. NumPy arrays provide a natural representation for scientific measurements.
2. Voltage, current, temperature, and device variables can be stored as arrays.
3. Related measurements must remain correctly paired.
4. Array lengths and shapes should be checked before analysis.
5. Units must be consistent before applying physical equations.
6. Resistance can be calculated using:
   $$
   R=\frac{V}{I}.
   $$
7. Power can be calculated using:
   $$
   P=VI.
   $$
8. Repeated measurements can be summarized using NumPy statistics.
9. Two-dimensional arrays can represent repeated measurements at multiple conditions.
10. `np.isnan()` can identify missing numerical values.
11. `np.isinf()` and `np.isfinite()` help identify invalid numerical values.
12. Semiconductor device data can be represented using paired and multivariable arrays.
13. Raw measurements should be preserved while transformed data is stored separately.
14. Calculated variables such as resistance and power can be added to an organized dataset.
15. NumPy provides the numerical bridge between experimental data, scientific calculations, and visualization.

---

12.46 Key Takeaway

> **NumPy becomes scientifically meaningful when arrays represent real observations and physical variables. The goal is not simply to perform calculations, but to preserve relationships, maintain units, validate measurements, derive useful quantities, and prepare reliable data for scientific interpretation and visualization.**

The next chapter introduces **Matplotlib fundamentals**, where these prepared scientific arrays will be converted into clear graphs and figures.
