# Chapter 17 — Importing and Exporting Data

## 17.1 Introduction

In real scientific work, data are usually not entered manually into a Python program.

Experimental data may be stored in:

- CSV files
- Excel spreadsheets
- Text files
- Data acquisition systems
- Laboratory instruments

Python allows us to import these data, analyze them, and save the results for future use.

The basic workflow is:

```text
Experimental Measurement
          ↓
      Data File
          ↓
       Python
          ↓
       Analysis
          ↓
    Results / Graph
          ↓
    Exported Data
```

In this chapter, we will learn how to work with **CSV files and spreadsheet data** using Python.

---

## 17.2 What Is a CSV File?

CSV stands for **Comma-Separated Values**.

A CSV file stores data in rows and columns.

For example, a diode dataset may be stored as:

```text
Voltage_V,Current_mA
0.10,0.01
0.20,0.02
0.30,0.05
0.40,0.15
0.50,1.20
0.60,8.50
```

The first row contains the **column names**.

Each following row contains one observation.

CSV files are commonly used because they are:

- simple
- lightweight
- easy to read
- supported by many software tools
- suitable for scientific data exchange

---

## 17.3 Creating a Small CSV Dataset

Create a file named:

```text
diode_iv.csv
```

and enter:

```text
Voltage_V,Current_mA
0.10,0.01
0.20,0.02
0.30,0.05
0.40,0.15
0.50,1.20
0.60,8.50
```

Save the file in a folder such as:

```text
unit_2/data/ch17/
```

The folder structure can be:

```text
scientific_python_and_data_skills/
│
├── unit_2/
│   └── data/
│       └── ch17/
│           └── diode_iv.csv
│
└── unit_2/
    └── ch17_importing_exporting_data.md
```

---

## 17.4 Why Use pandas?

**pandas** is a Python library designed for working with structured and tabular data.

It is particularly useful for:

- CSV files
- Excel files
- tables
- data cleaning
- filtering
- organizing data
- basic data analysis

We normally import pandas as:

```python
import pandas as pd
```

The abbreviation `pd` is the commonly used name for pandas.

---

## 17.5 Reading a CSV File

We can read our CSV file using:

```python
import pandas as pd

data = pd.read_csv("diode_iv.csv")
```

Now the contents of the CSV file are stored in the variable `data`.

We can display the complete dataset:

```python
print(data)
```

The output will look similar to:

```text
   Voltage_V  Current_mA
0       0.10        0.01
1       0.20        0.02
2       0.30        0.05
3       0.40        0.15
4       0.50        1.20
5       0.60        8.50
```

---

## 17.6 Understanding the DataFrame

When pandas reads a table, it creates a **DataFrame**.

A DataFrame is a two-dimensional table containing rows and columns.

For example:

```text
           DataFrame
     ┌───────────────┐
     │ Voltage Current│
     │  0.10    0.01 │
     │  0.20    0.02 │
     │  0.30    0.05 │
     │  ...      ... │
     └───────────────┘
```

We can think of a DataFrame as a scientific data table that Python can manipulate.

---

## 17.7 Inspecting the Data

After importing data, we should inspect it before performing calculations.

### Display the first few rows

```python
print(data.head())
```

`head()` displays the first five rows by default.

### Display the last few rows

```python
print(data.tail())
```

### Find the number of rows and columns

```python
print(data.shape)
```

For our dataset:

```text
(6, 2)
```

This means:

- 6 rows
- 2 columns

### Display column names

```python
print(data.columns)
```

---

## 17.8 Selecting a Column

We can select one column using its column name.

```python
voltage = data["Voltage_V"]
```

Similarly:

```python
current = data["Current_mA"]
```

Now we can work with these variables separately.

For example:

```python
print(voltage)
```

or:

```python
print(current)
```

---

## 17.9 Basic Calculations

Once the data are imported, we can perform calculations.

For example, find the maximum current:

```python
maximum_current = data["Current_mA"].max()

print(maximum_current)
```

Output:

```text
8.5
```

Find the minimum current:

```python
minimum_current = data["Current_mA"].min()

print(minimum_current)
```

Find the average current:

```python
average_current = data["Current_mA"].mean()

print(average_current)
```

This demonstrates an important scientific workflow:

```text
CSV file
   ↓
pandas
   ↓
DataFrame
   ↓
Calculation
   ↓
Scientific result
```

---

## 17.10 Plotting Imported Data

Imported data can be directly used for visualization.

```python
import pandas as pd
import matplotlib.pyplot as plt

data = pd.read_csv("diode_iv.csv")

plt.plot(data["Voltage_V"], data["Current_mA"], marker="o")

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Diode I-V Characteristics")

plt.show()
```

Here:

- `Voltage_V` is used for the x-axis.
- `Current_mA` is used for the y-axis.

This is one of the most common workflows in scientific data analysis.

---

## 17.11 File Paths

Python needs to know where the data file is located.

If the CSV file is in the same folder as the Python program:

```python
data = pd.read_csv("diode_iv.csv")
```

If it is inside a folder:

```python
data = pd.read_csv("data/diode_iv.csv")
```

For larger projects, organizing files into folders is useful.

For example:

```text
project/
│
├── data/
│   └── diode_iv.csv
│
├── notebooks/
│   └── analysis.ipynb
│
└── results/
```

Good file organization makes scientific projects easier to understand and reproduce.

---

## 17.12 Working with Excel Files

Scientific data are also commonly stored in Excel spreadsheets.

For example:

```text
diode_measurements.xlsx
```

pandas can read Excel data using:

```python
import pandas as pd

data = pd.read_excel("diode_measurements.xlsx")
```

The resulting object is again a DataFrame.

We can then use:

```python
print(data.head())
```

and perform calculations just as we did with CSV data.

> **Note:** Reading Excel files may require an additional Python package such as `openpyxl`.

---

## 17.13 Importing a Specific Excel Sheet

An Excel workbook may contain multiple sheets.

For example:

```text
Sheet 1 → Diode
Sheet 2 → Zener
Sheet 3 → Temperature
```

We can specify the sheet:

```python
data = pd.read_excel(
    "device_measurements.xlsx",
    sheet_name="Diode"
)
```

This allows us to work with only the required dataset.

---

## 17.14 Exporting Data to CSV

After analysis, we may create new calculated variables.

For example:

```python
data["Power_mW"] = (
    data["Voltage_V"] * data["Current_mA"]
)
```

Because:

$$
P=VI
$$

When voltage is in volts and current is in milliamperes, the result is in milliwatts.

We can now save the updated dataset:

```python
data.to_csv("diode_with_power.csv", index=False)
```

The new CSV file contains the original measurements plus the calculated power.

---

## 17.15 Why `index=False`?

A pandas DataFrame has an internal row index.

For example:

```text
0
1
2
3
4
5
```

If we export without specifying `index=False`, pandas may save this index as an additional column.

Using:

```python
data.to_csv("diode_with_power.csv", index=False)
```

prevents the DataFrame index from being written to the CSV file.

---

## 17.16 Exporting to Excel

We can also save a DataFrame as an Excel file.

```python
data.to_excel(
    "diode_with_power.xlsx",
    index=False
)
```

This is useful when the results need to be shared with someone who works primarily with spreadsheet software.

---

## 17.17 A Complete Example

Let's combine the steps.

### Step 1 — Import pandas

```python
import pandas as pd
```

### Step 2 — Read the CSV file

```python
data = pd.read_csv("diode_iv.csv")
```

### Step 3 — Inspect the data

```python
print(data.head())
print(data.shape)
```

### Step 4 — Calculate power

```python
data["Power_mW"] = (
    data["Voltage_V"] * data["Current_mA"]
)
```

### Step 5 — Display the result

```python
print(data)
```

### Step 6 — Save the new dataset

```python
data.to_csv(
    "diode_with_power.csv",
    index=False
)
```

### Step 7 — Plot the original measurements

```python
import matplotlib.pyplot as plt

plt.plot(
    data["Voltage_V"],
    data["Current_mA"],
    marker="o"
)

plt.xlabel("Voltage (V)")
plt.ylabel("Current (mA)")
plt.title("Diode I-V Characteristics")

plt.show()
```

The complete workflow is:

```text
CSV file
   ↓
Read with pandas
   ↓
Inspect data
   ↓
Calculate new quantity
   ↓
Visualize
   ↓
Export results
```

---

## 17.18 Importing Data from a Research Dataset

In scientific work, datasets may come from:

- Research papers
- Public data repositories
- Laboratory instruments
- Government databases
- Manufacturer measurements
- University experiments

The important point is that the dataset should be accompanied by information about:

- source
- variables
- units
- experimental conditions
- measurement method

For example:

```text
Dataset: Diode I-V measurements
Source: Experimental study
Variables: Voltage, Current
Voltage unit: V
Current unit: mA
```

This information helps us understand the data before analysis.

---

## 17.19 Common Problems When Importing Data

### Problem 1 — Incorrect file path

```python
FileNotFoundError
```

This usually means Python cannot find the file.

Check:

- file name
- folder location
- file extension
- current working directory

### Problem 2 — Incorrect column name

Suppose the actual column is:

```text
Voltage_V
```

but we write:

```python
data["Voltage"]
```

Python will not find the requested column.

Always inspect:

```python
print(data.columns)
```

### Problem 3 — Missing values

A dataset may contain empty cells.

For example:

```text
Voltage_V,Current_mA
0.10,0.01
0.20,
0.30,0.05
```

The missing value must be identified before analysis.

We will study this in detail in **Chapter 18**.

---

## 17.20 Key Points

- Scientific data are often stored outside Python.
- CSV is a common format for scientific data.
- pandas provides convenient tools for importing and exporting data.
- `pd.read_csv()` reads a CSV file.
- `pd.read_excel()` reads an Excel file.
- Imported tabular data are stored in a pandas DataFrame.
- `head()` helps inspect the first rows.
- `shape` gives the number of rows and columns.
- `columns` shows column names.
- DataFrame columns can be selected using their names.
- `to_csv()` exports data to CSV.
- `to_excel()` exports data to Excel.
- Good file organization is important for reproducible scientific work.

---

## 17.21 Quick Practice

### Question 1

What does CSV stand for?

### Question 2

Which pandas function is used to read a CSV file?

### Question 3

What is a DataFrame?

### Question 4

What does `data.shape` tell us?

### Question 5

Why do we use `index=False` when exporting a DataFrame?

---

## 17.22 Hands-on Activity

Create a file named:

```text
resistor_data.csv
```

with the following data:

| Voltage_V | Current_A |
|---:|---:|
| 1.0 | 0.010 |
| 2.0 | 0.020 |
| 3.0 | 0.030 |
| 4.0 | 0.040 |
| 5.0 | 0.050 |

Using pandas:

1. Import the CSV file.
2. Display the first five rows.
3. Find the shape of the dataset.
4. Find the maximum current.
5. Calculate resistance using:

$$
R=\frac{V}{I}
$$

6. Store resistance in a new column.
7. Export the updated dataset as `resistor_with_resistance.csv`.
8. Plot voltage against current.

---

## 17.23 Chapter Summary

Scientific data are often collected and stored outside Python. Python allows us to bring these datasets into our analysis workflow.

The basic process is:

$$
\boxed{
   {Data File}
\rightarrow
   {Import}
\rightarrow
   {Inspect}
\rightarrow
   {Analyze}
\rightarrow
   {Visualize}
\rightarrow
   {Export}
}
$$

In the next chapter, we will learn how to **clean and organize experimental data before analysis**.
