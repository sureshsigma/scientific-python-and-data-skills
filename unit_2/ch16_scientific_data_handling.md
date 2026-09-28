# Chapter 16 — Scientific Data Handling

## 16.1 Introduction

In scientific experiments, we collect measurements such as:

- Voltage
- Current
- Temperature
- Resistance
- Time
- Light intensity
- Pressure
- Semiconductor device characteristics

The collected measurements are called **scientific data**.

For example, while studying a diode, we may apply different voltages and measure the corresponding current.

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.10 | 0.01 |
| 0.20 | 0.02 |
| 0.30 | 0.05 |
| 0.40 | 0.15 |
| 0.50 | 1.20 |
| 0.60 | 8.50 |

This table is an example of **experimental scientific data**.

The purpose of scientific data analysis is not simply to store these numbers. We want to use the data to answer scientific questions.

For example:

> How does diode current change with applied voltage?

---

## 16.2 What Is Scientific Data?

**Scientific data** are measurements or observations collected during an experiment, simulation, observation, or scientific investigation.

Examples:

```python
Temperature = 300  # K
Voltage = 0.50     # V
Current = 1.20     # mA
Resistance = 100   # ohm
```

Scientific data usually contain:

- measurements
- units
- experimental conditions
- observations
- calculated quantities

### Example

Suppose we measure the current through a resistor at different voltages.

| Voltage (V) | Current (A) |
|---:|---:|
| 1 | 0.010 |
| 2 | 0.020 |
| 3 | 0.030 |
| 4 | 0.040 |

Here:

- Voltage is a measured variable.
- Current is a measured variable.
- Volt (V) is the unit of voltage.
- Ampere (A) is the unit of current.

---

## 16.3 Variables in Experimental Data

A **variable** is a quantity whose value can change during an experiment.

For example, in a diode experiment:

- Applied voltage can change.
- Current changes in response.

We generally distinguish between two important variables.

### Independent Variable

The quantity that we control or change during the experiment.

Example:

**Voltage**

### Dependent Variable

The quantity that we measure as a response.

Example:

**Current**

Therefore:

$$
\text{Voltage} \rightarrow \text{Independent variable}
$$

$$
\text{Current} \rightarrow \text{Dependent variable}
$$

For a diode I–V experiment:

```text
Applied Voltage
       ↓
     Diode
       ↓
Measured Current
```

This relationship is important when creating scientific plots.

---

## 16.4 Observations

An **observation** is one recorded measurement from an experiment.

For example:

```text
Voltage = 0.40 V
Current = 0.15 mA
```

This represents one observation.

If we record measurements at six different voltages, we have six observations.

| Observation | Voltage (V) | Current (mA) |
|---:|---:|---:|
| 1 | 0.10 | 0.01 |
| 2 | 0.20 | 0.02 |
| 3 | 0.30 | 0.05 |
| 4 | 0.40 | 0.15 |
| 5 | 0.50 | 1.20 |
| 6 | 0.60 | 8.50 |

Each row represents one observation.

---

## 16.5 Rows and Columns

Scientific datasets are commonly organized in a table.

### Rows

A row generally represents **one observation**.

### Columns

A column generally represents **one variable**.

For example:

| Voltage | Current | Temperature |
|---:|---:|---:|
| 0.10 | 0.01 | 25 |
| 0.20 | 0.02 | 25 |
| 0.30 | 0.05 | 25 |

Here:

- Each row = one measurement
- Voltage = variable
- Current = variable
- Temperature = variable

This structure is extremely useful because Python libraries such as **NumPy** and **pandas** can work directly with tabular scientific data.

---

## 16.6 Why Units Are Important

A number without a unit may be meaningless or misleading.

For example:

```text
Voltage = 5
```

does not clearly tell us whether the value is:

- 5 V
- 5 mV
- 5 kV

Therefore, scientific datasets should clearly specify units.

### Example

```text
Voltage (V)
Current (mA)
Temperature (°C)
Resistance (Ω)
```

### Unit Consistency

Suppose we have:

```text
Voltage = 500 mV
```

and another measurement:

```text
Voltage = 0.6 V
```

Before analysis, we should use a consistent unit.

Since:

$$
500\text{ mV}=0.5\text{ V}
$$

we can write:

```text
0.5 V
0.6 V
```

This becomes particularly important when performing calculations using Python.

---

## 16.7 Experimental Conditions

Measurements depend on experimental conditions.

For semiconductor experiments, important conditions may include:

- Temperature
- Applied voltage
- Applied current
- Light intensity
- Device type
- Measurement time

For example, a solar cell's current-voltage characteristics can change significantly with illumination.

Therefore, a useful dataset might contain:

| Voltage (V) | Current (mA) | Temperature (°C) | Irradiance (W/m²) |
|---:|---:|---:|---:|
| 0.00 | 5.20 | 25 | 1000 |
| 0.10 | 5.10 | 25 | 1000 |
| 0.20 | 4.80 | 25 | 1000 |

The additional information helps us understand **under what conditions the measurements were obtained**.

---

## 16.8 From Experiment to Dataset

A scientific experiment usually follows this process:

```text
Experiment
    ↓
Measurement
    ↓
Data Recording
    ↓
Data Table
    ↓
CSV / Spreadsheet
    ↓
Python
    ↓
Analysis
    ↓
Graph
    ↓
Scientific Interpretation
```

For example:

```text
Measure diode voltage and current
            ↓
      Record measurements
            ↓
       Create a table
            ↓
          Save CSV
            ↓
      Read using Python
            ↓
       Plot I–V curve
            ↓
      Study diode behaviour
```

This workflow will be used throughout **Unit II**.

---

## 16.9 A Small Scientific Dataset

Let's consider the following diode dataset.

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.10 | 0.01 |
| 0.20 | 0.02 |
| 0.30 | 0.05 |
| 0.40 | 0.15 |
| 0.50 | 1.20 |
| 0.60 | 8.50 |

We can store this data in Python using NumPy.

```python
import numpy as np

voltage = np.array([0.10, 0.20, 0.30, 0.40, 0.50, 0.60])
current = np.array([0.01, 0.02, 0.05, 0.15, 1.20, 8.50])
```

Now Python contains our experimental data.

---

## 16.10 Inspecting the Data

We can examine the data using simple NumPy operations.

```python
print(voltage)
print(current)
```

We can find the number of measurements:

```python
print(len(voltage))
```

Output:

```text
6
```

Therefore, the dataset contains **6 observations**.

We can also find the minimum and maximum voltage:

```python
print(np.min(voltage))
print(np.max(voltage))
```

Output:

```text
0.1
0.6
```

So the voltage range is:

$$
0.1\text{ V} \leq V \leq 0.6\text{ V}
$$

---

## 16.11 Basic Scientific Questions

Once the data are available, we should not immediately start calculating.

First ask:

### Question 1

What variables are present?

**Answer:** Voltage and current.

### Question 2

Which variable is independent?

**Answer:** Voltage.

### Question 3

Which variable is dependent?

**Answer:** Current.

### Question 4

What are their units?

**Answer:**

- Voltage → V
- Current → mA

### Question 5

What relationship are we studying?

**Answer:**

$$
I=f(V)
$$

That means current is being studied as a function of voltage.

---

## 16.12 Visualizing Scientific Data

A simple plot can help us understand the relationship between variables.

```python
import matplotlib.pyplot as plt

plt.plot(voltage, current, marker='o')

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Diode I-V Characteristics")

plt.show()
```

The graph helps us see how current changes as voltage increases.

In the dataset, current increases slowly at first and then increases rapidly.

This is an example of how **visualization helps us understand scientific behaviour**.

---

## 16.13 Why Data Handling Matters

Poorly organized data can produce incorrect scientific conclusions.

For example, suppose voltage is recorded in volts:

```text
0.1
0.2
0.3
```

but current is recorded sometimes in amperes and sometimes in milliamperes:

```text
0.01 A
0.02 mA
0.05 A
```

If we do not identify the units, our analysis may be incorrect.

Therefore, before performing calculations we should always check:

- Variable names
- Units
- Missing values
- Incorrect values
- Number of observations
- Experimental conditions
- Data format

These issues will be studied in more detail in **Chapter 18 — Cleaning and Organizing Experimental Data**.

---

## 16.14 Scientific Data and Python

Python provides several tools for scientific data analysis.

### NumPy

Used mainly for:

- Numerical arrays
- Mathematical operations
- Statistical calculations
- Numerical computation

### pandas

Used mainly for:

- Tabular data
- CSV files
- Data cleaning
- Data organization
- Data filtering

### Matplotlib

Used mainly for:

- Scientific graphs
- Experimental data visualization

### SciPy

Used mainly for:

- Scientific calculations
- Curve fitting
- Interpolation
- Integration
- Equation solving
- Signal processing

We will use these tools throughout Unit II.

---

## 16.15 Real Data vs Synthetic Data

Scientific datasets used in learning can come from two sources.

### Real Experimental Data

Data actually collected from an experiment or reported in a research study.

Examples:

- Measured diode I–V characteristics
- Measured solar-cell I–V characteristics
- Experimental semiconductor band-gap measurements

### Synthetic Data

Data generated artificially using a known mathematical or physical relationship.

For example:

$$
y=2x+1
$$

We can generate data from this equation and add some measurement-like variation.

Synthetic data are useful when we want to understand a particular concept without unnecessary complexity.

**In this unit, we will use both real and synthetic datasets and clearly identify which type is being used.**

---

## 16.16 Key Points

- Scientific data are measurements or observations obtained during scientific work.
- A variable is a quantity that can change.
- The independent variable is controlled or varied.
- The dependent variable is measured in response.
- Each row generally represents an observation.
- Each column generally represents a variable.
- Units must always be clearly identified.
- Experimental conditions are important when interpreting data.
- Scientific data should be checked before analysis.
- Python can be used to organize, analyze, visualize, and interpret scientific data.
- Scientific data analysis is more than performing calculations; the final goal is **scientific interpretation**.

---

## 16.17 Quick Practice

### Question 1

In a diode I–V experiment, which is generally the independent variable?

### Question 2

What does one row in an experimental dataset generally represent?

### Question 3

Why are units important in scientific data?

### Question 4

Identify the independent and dependent variables:

> The current through a resistor is measured for different applied voltages.

### Question 5

What is the purpose of plotting experimental data?

---

## 16.18 Hands-on Activity

Consider the following measurements:

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.10 | 0.01 |
| 0.20 | 0.02 |
| 0.30 | 0.05 |
| 0.40 | 0.15 |
| 0.50 | 1.20 |
| 0.60 | 8.50 |

Using Python:

1. Create NumPy arrays for voltage and current.
2. Find the number of observations.
3. Find the minimum and maximum voltage.
4. Plot current against voltage.
5. Label both axes with units.
6. Write two observations about the graph.

---

## 16.19 Chapter Summary

Scientific experiments produce measurements. To obtain useful information from those measurements, we need to organize the data properly, understand the variables and units, visualize the measurements, and then perform scientific analysis.

The basic workflow is:

$$
\boxed{\text{Experiment}\rightarrow\text{Data}\rightarrow\text{Python}\rightarrow\text{Analysis}\rightarrow\text{Interpretation}}
$$

In the next chapter, we will learn how to **import experimental data from CSV files and spreadsheets into Python**.
