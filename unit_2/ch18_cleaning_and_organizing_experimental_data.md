# Chapter 18 — Cleaning and Organizing Experimental Data

## 18.1 Introduction

Experimental data is rarely ready for analysis immediately.

Data collected from a laboratory experiment may contain:

- missing values
- incorrect units
- extra spaces or symbols
- repeated measurements
- unusual values
- inconsistent formatting
- values recorded in the wrong column

Before performing scientific calculations, the data should be **cleaned, organized, and checked**.

A useful workflow is:

$$
\boxed{
\text{Raw Data}
\rightarrow
\text{Clean}
\rightarrow
\text{Organize}
\rightarrow
\text{Check}
\rightarrow
\text{Analyze}
}
$$

In this chapter, we use Python and pandas to prepare experimental data for analysis.

---

## 18.2 Example: Diode Experimental Data

Consider an experiment in which the voltage across a diode and the corresponding current are measured.

Suppose the observations are recorded as:

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.10 | 0.02 |
| 0.20 | 0.04 |
| 0.30 | 0.08 |
| 0.40 | 0.15 |
| 0.50 | 0.30 |
| 0.60 | 0.65 |

A clean dataset should have:

- meaningful column names
- correct units
- one observation per row
- numerical values stored as numbers
- no unnecessary text inside numerical columns

---

## 18.3 Loading Experimental Data

Suppose the data is stored in a CSV file called `diode_data.csv`.

```python
import pandas as pd

data = pd.read_csv("diode_data.csv")

print(data)
```

Before analysis, inspect the dataset.

```python
print(data.head())
print(data.info())
print(data.shape)
```

### What do these commands tell us?

- `head()` shows the first few observations.
- `info()` shows column names, data types, and missing values.
- `shape` gives the number of rows and columns.

For example:

```text
(6, 2)
```

means 6 observations and 2 variables.

---

## 18.4 Checking Column Names

Column names should be simple and meaningful.

For example:

```text
Voltage (V)
Current (mA)
```

may be useful for a report, but simpler names are often easier to use in Python.

```python
data.columns = ["voltage_V", "current_mA"]

print(data.columns)
```

Now a calculation can be written clearly:

```python
print(data["voltage_V"])
```

Good column names reduce confusion during analysis.

---

## 18.5 Checking Data Types

Experimental data should have appropriate data types.

```python
print(data.dtypes)
```

A typical result may be:

```text
voltage_V     float64
current_mA    float64
dtype: object
```

`float64` indicates numerical decimal data.

If a numerical column has been imported as text, calculations may fail.

For example:

```python
data["voltage_V"] = pd.to_numeric(data["voltage_V"])
```

If some entries are invalid, we can use:

```python
data["voltage_V"] = pd.to_numeric(
    data["voltage_V"],
    errors="coerce"
)
```

Invalid values are converted to `NaN`.

---

## 18.6 Missing Values

Experimental measurements may sometimes be missing.

For example:

| Voltage (V) | Current (mA) |
|---:|---:|
| 0.10 | 0.02 |
| 0.20 | 0.04 |
| 0.30 | NaN |
| 0.40 | 0.15 |

`NaN` means that the value is missing or not available.

We can check missing values using:

```python
print(data.isnull().sum())
```

Example:

```text
voltage_V     0
current_mA    1
dtype: int64
```

This tells us that one current measurement is missing.

### Why is this important?

A missing measurement should not simply be replaced without understanding why it is missing.

Possible actions include:

- rechecking the original laboratory record
- repeating the experiment
- removing the observation when appropriate
- using an interpolation or estimation method when scientifically justified

The correct choice depends on the experiment.

---

## 18.7 Removing Rows with Missing Values

If a missing observation should not be used in the analysis:

```python
clean_data = data.dropna()

print(clean_data)
```

This removes rows containing missing values.

We should not automatically remove every missing value. First determine whether the missing observation has scientific significance.

---

## 18.8 Removing Duplicate Measurements

Sometimes the same observation may accidentally be entered twice.

Check for duplicate rows:

```python
print(data.duplicated())
```

Count duplicates:

```python
print(data.duplicated().sum())
```

Remove duplicate rows:

```python
data = data.drop_duplicates()
```

Duplicate removal should be done carefully.

Two identical measurements may be legitimate repeated experimental observations. Therefore, we should distinguish between:

**accidental duplicate records**

and

**intentional repeated measurements**.

---

## 18.9 Checking for Impossible Values

Scientific data should also be checked for physically impossible or clearly incorrect values.

For example, suppose a voltage dataset contains:

```text
0.10
0.20
0.30
-15.00
0.50
```

If the experiment cannot produce a negative voltage, `-15.00` may indicate a recording error.

We can inspect unusual values:

```python
print(data["voltage_V"].describe())
```

The `describe()` method gives basic statistics such as:

- count
- mean
- standard deviation
- minimum
- maximum
- quartiles

Example:

```python
print(data.describe())
```

This is a useful first check before scientific analysis.

---

## 18.10 Checking Units

Units are extremely important in scientific computing.

Suppose current is recorded in milliamperes:

$$
I = 0.65\ \text{mA}
$$

For calculations requiring amperes:

$$
0.65\ \text{mA}
=
0.65\times10^{-3}\ \text{A}
$$

In Python:

```python
data["current_A"] = data["current_mA"] * 1e-3
```

Now both representations are available:

```text
current_mA
current_A
```

Keeping units explicit helps prevent numerical errors.

---

## 18.11 Formatting Numerical Values

Sometimes measurements contain more decimal places than are meaningful.

For example:

```text
0.100000
0.200000
0.300000
```

For display, we may use:

```python
print(data.round(3))
```

This changes the displayed precision.

### Important

Rounding should normally be used for **presentation**, not to unnecessarily alter the original experimental measurements.

Keep the original data whenever possible.

---

## 18.12 Creating a Calculated Variable

Experimental datasets often require derived quantities.

For a resistor experiment:

$$
R = \frac{V}{I}
$$

Suppose voltage is in volts and current is in amperes.

```python
data["resistance_ohm"] = (
    data["voltage_V"] / data["current_A"]
)

print(data)
```

A new column has now been created from the experimental measurements.

This is an important idea in scientific data analysis:

> Raw measurements can be transformed into scientifically meaningful variables.

---

## 18.13 Sorting Experimental Data

Measurements may not always be entered in the correct order.

For example, voltage values may appear as:

```text
0.5
0.1
0.3
0.2
0.4
```

Sort the data by voltage:

```python
data = data.sort_values("voltage_V")

print(data)
```

Sorted data is particularly useful when creating scientific plots.

---

## 18.14 Organizing a Dataset

A well-organized experimental dataset generally follows these principles:

1. One row represents one observation.
2. One column represents one variable.
3. Column names are meaningful.
4. Units are clearly documented.
5. Numerical values are stored as numbers.
6. Missing values are identified.
7. Original observations are preserved.
8. Derived variables are clearly named.

For example:

| voltage_V | current_mA | current_A |
|---:|---:|---:|
| 0.10 | 0.02 | 0.00002 |
| 0.20 | 0.04 | 0.00004 |
| 0.30 | 0.08 | 0.00008 |
| 0.40 | 0.15 | 0.00015 |

This format is easy to analyze using Python.

---

## 18.15 A Simple Cleaning Workflow

A practical cleaning workflow is:

```python
import pandas as pd

# 1. Read the data
data = pd.read_csv("diode_data.csv")

# 2. Inspect the data
print(data.head())
print(data.info())

# 3. Rename columns
data.columns = ["voltage_V", "current_mA"]

# 4. Convert columns to numeric
data["voltage_V"] = pd.to_numeric(
    data["voltage_V"],
    errors="coerce"
)

data["current_mA"] = pd.to_numeric(
    data["current_mA"],
    errors="coerce"
)

# 5. Check missing values
print(data.isnull().sum())

# 6. Remove invalid rows if appropriate
data = data.dropna()

# 7. Remove accidental duplicates
data = data.drop_duplicates()

# 8. Create current in amperes
data["current_A"] = data["current_mA"] * 1e-3

# 9. Sort by voltage
data = data.sort_values("voltage_V")

print(data)
```

This produces a dataset that is much easier to analyze.

---

## 18.16 Why Cleaning Comes Before Analysis

Suppose we calculate the mean current before checking the data.

If the dataset contains:

- a missing value
- a duplicated measurement
- an incorrect unit
- a typing error

the calculated result may be misleading.

Therefore:

$$
\boxed{
\text{Clean Data}
\rightarrow
\text{Reliable Analysis}
\rightarrow
\text{Meaningful Scientific Conclusion}
}
$$

Python can perform calculations very quickly, but it cannot automatically determine whether every experimental measurement makes scientific sense.

Scientific judgment is still required.

---

## 18.17 Scientific Data vs Computer Data

A computer sees:

```text
0.5
```

A scientist asks:

> 0.5 what?

It could be:

- 0.5 V
- 0.5 A
- 0.5 mA
- 0.5 Ω
- 0.5 nm

Therefore, scientific data should always be interpreted together with:

- variable name
- unit
- experimental condition
- measurement method

---

## 18.18 Key Points

- Experimental data should be cleaned before analysis.
- `pandas` is useful for organizing scientific datasets.
- `head()`, `info()`, `shape()`, and `describe()` help inspect data.
- Missing values can be detected using `isnull()`.
- Duplicate records can be checked using `duplicated()`.
- Numerical columns should have appropriate numerical data types.
- Units must be checked before calculations.
- Derived quantities can be created as new columns.
- Sorting data is useful for analysis and plotting.
- Original experimental data should be preserved whenever possible.
- Data cleaning requires scientific judgment.

---

## 18.19 Quick Practice

### Practice 1

What does the following command do?

```python
data.head()
```

### Practice 2

Write Python code to check missing values in a DataFrame called `data`.

### Practice 3

Suppose current is stored in mA. Write Python code to create a new column containing current in A.

### Practice 4

Why should units be checked before performing scientific calculations?

### Practice 5

What is the difference between an accidental duplicate measurement and a legitimate repeated experimental measurement?

---

## 18.20 Hands-on Activity

Create a small dataset containing experimental measurements of voltage and current.

Your dataset should contain at least 8 observations.

Perform the following steps:

1. Save the data as a CSV file.
2. Import it using pandas.
3. Display the first five observations.
4. Check the data types.
5. Check for missing values.
6. Check for duplicate rows.
7. Sort the data by voltage.
8. Convert current from mA to A.
9. Calculate resistance using

$$
R=\frac{V}{I}
$$

10. Display the cleaned dataset.

### Scientific Question

After cleaning the data, ask:

> Does the calculated resistance remain approximately constant for all observations?

This connects data cleaning with the next stage of scientific data analysis.

---

## 18.21 Chapter Summary

Scientific data analysis begins with reliable data.

The basic process is:

$$
\boxed{
\text{Collect}
\rightarrow
\text{Clean}
\rightarrow
\text{Organize}
\rightarrow
\text{Check}
\rightarrow
\text{Analyze}
}
$$

In this chapter, pandas was used to clean and organize experimental data. These skills will be used in later chapters for scientific analysis, curve fitting, interpolation, numerical calculations, and semiconductor device characterization.
