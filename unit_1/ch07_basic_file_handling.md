# Chapter 7 — Basic File Handling

## 7.1 Learning Objectives

After completing this chapter, you should be able to:

- explain why scientific programs need to read and write files;
- understand the difference between text files and structured data files;
- open and close files safely using Python;
- read text files using common file methods;
- write results to text files;
- use `with open(...)` for safe file handling;
- work with CSV files using Python's built-in `csv` module;
- read numerical experimental data from a CSV file;
- write calculated results to a CSV file;
- understand the role of file handling in reproducible scientific analysis.

---

7.2 Why Do We Need Files?

Until now, most of our programs have used data entered directly into Python.

For example:

```python
voltages = [1, 2, 3, 4, 5]
```

This is useful for learning, but real experiments usually generate data outside the Python program.

A measurement instrument may produce a file such as:

```text
experiment_01.csv
```

A laboratory may provide:

```text
temperature_data.csv
```

A spreadsheet may contain:

```text
voltage,current
0.1,0.001
0.2,0.002
0.3,0.004
```

A scientific program therefore needs to be able to:

1. read data from files;
2. process the data;
3. calculate results;
4. save the results.

The workflow becomes:

```text
Experiment
    ↓
Data file
    ↓
Python
    ↓
Analysis
    ↓
Result file
```

This is a fundamental part of scientific data analysis.

---

7.3 Files and Persistence

Variables exist while a Python program is running.

For example:

```python
temperature = 300
```

If the program ends, the variable is no longer available.

A file provides a way to store information so that it can be used later.

This is called **persistent storage**.

For example:

```text
Python program
     ↓
save results
     ↓
results.txt
     ↓
open later
     ↓
continue analysis
```

File handling therefore connects one analysis session with another.

---

7.4 Text Files

A text file contains characters that can be read by humans and programs.

For example:

```text
Experiment: Diode Test
Temperature: 300 K
Voltage: 0.70 V
Current: 0.0025 A
```

A text file may have extensions such as:

```text
.txt
.md
.csv
```

Although these files can all contain text, their structure and intended use may differ.

---

7.5 Opening a File

Python provides the `open()` function for working with files.

For example:

```python
file = open("data.txt", "r")
```

The second argument specifies the mode.

The most common modes are:

| Mode | Meaning |
| --- | --- |
| `"r"` | Read |
| `"w"` | Write |
| `"a"` | Append |

When reading a file, use:

```python
"r"
```

When creating or replacing a file, use:

```python
"w"
```

When adding content to an existing file, use:

```python
"a"
```

---

7.6 Reading a Text File

Suppose `data.txt` contains:

```text
Silicon diode
Temperature = 300 K
Voltage = 0.7 V
```

We can read the file using:

```python
file = open("data.txt", "r")

content = file.read()

print(content)

file.close()
```

Output:

```text
Silicon diode
Temperature = 300 K
Voltage = 0.7 V
```

The `read()` method reads the contents of the file.

The `close()` method closes the file.

---

7.7 Why Should Files Be Closed?

Opening a file creates a connection between the Python program and the file.

After the required operation is complete, the connection should be closed.

The traditional pattern is:

```python
file = open("data.txt", "r")

content = file.read()

file.close()
```

However, there is a safer and more convenient approach.

Python provides the `with` statement.

---

7.8 The `with open()` Pattern

The recommended approach is:

```python
with open("data.txt", "r") as file:
    content = file.read()

print(content)
```

The file is automatically closed when the `with` block ends.

This is the preferred pattern for most basic file operations.

The structure is:

```python
with open(filename, mode) as file:
    statements
```

The indentation identifies the statements that operate on the file.

---

7.9 Reading Lines

The `readlines()` method reads the file as a list of lines.

For example:

```python
with open("data.txt", "r") as file:
    lines = file.readlines()

print(lines)
```

If the file contains:

```text
10
20
30
```

the result is approximately:

```text
['10\n', '20\n', '30\n']
```

The `\n` represents a newline character.

We can process each line using a loop:

```python
with open("data.txt", "r") as file:
    for line in file:
        print(line.strip())
```

The `strip()` method removes leading and trailing whitespace, including the newline character.

---

7.10 Writing a Text File

To write to a file, use mode `"w"`.

For example:

```python
with open("results.txt", "w") as file:
    file.write("Scientific Python\n")
    file.write("Semiconductor Science\n")
```

The file `results.txt` will contain:

```text
Scientific Python
Semiconductor Science
```

The `\n` creates a new line.

---

7.11 Writing Scientific Results

Suppose we calculate current using Ohm's law:

$$
I=\frac{V}{R}.
$$

We can save the result to a text file.

```python
V = 5
R = 100
I = V / R

with open("result.txt", "w") as file:
    file.write(f"Voltage = {V} V\n")
    file.write(f"Resistance = {R} ohm\n")
    file.write(f"Current = {I} A\n")
```

The file will contain:

```text
Voltage = 5 V
Resistance = 100 ohm
Current = 0.05 A
```

This provides a simple way to preserve calculation results.

---

7.12 The Difference Between `"w"` and `"a"`

The `"w"` mode writes a new file or replaces the contents of an existing file.

For example:

```python
with open("results.txt", "w") as file:
    file.write("First result\n")
```

If the file already contains information, `"w"` replaces it.

The `"a"` mode appends new information.

```python
with open("results.txt", "a") as file:
    file.write("Second result\n")
```

The existing content remains, and the new line is added at the end.

Be careful when using `"w"` because it can overwrite existing results.

---

7.13 File Paths

A file can be located in the current working directory or in another folder.

For example:

```python
with open("data.txt", "r") as file:
    ...
```

looks for `data.txt` in the current working directory.

A relative path can specify a folder:

```python
with open("data/experiment.txt", "r") as file:
    ...
```

On Windows, file paths can require special handling.

For example, a raw string can be used:

```python
path = r"D:\Experiments\data.txt"
```

The `r` before the string tells Python to treat it as a raw string.

For portable scientific projects, relative paths are generally preferable when possible.

---

7.14 File Existence

If Python attempts to open a file that does not exist:

```python
with open("missing_file.txt", "r") as file:
    content = file.read()
```

Python will produce a `FileNotFoundError`.

This is not necessarily a programming mistake.

It may simply mean:

- the file name is incorrect;
- the file is in another folder;
- the path is incorrect;
- the file has not yet been created.

Understanding file paths is therefore an important practical skill.

---

7.15 CSV Files

Experimental data is often stored in **CSV** format.

CSV means:

> Comma-Separated Values.

A simple CSV file may contain:

```text
Voltage,Current
0.1,0.001
0.2,0.002
0.3,0.004
0.4,0.008
```

The first line contains column headings.

Each following line represents an observation.

Conceptually:

| Voltage | Current |
| ---: | ---: |
| 0.1 | 0.001 |
| 0.2 | 0.002 |
| 0.3 | 0.004 |
| 0.4 | 0.008 |

CSV is a simple and widely used format for transferring tabular data between instruments, spreadsheets, and programming environments.

---

7.16 Why CSV Is Important for This Course

Semiconductor and electronics experiments can produce data such as:

- voltage;
- current;
- temperature;
- time;
- resistance;
- sensor response.

A laboratory instrument may export this information as a CSV file.

Python can then read the file and perform analysis.

The workflow is:

```text
Instrument
    ↓
CSV file
    ↓
Python
    ↓
Data cleaning
    ↓
Calculation
    ↓
Visualization
```

More advanced CSV handling will later be performed using scientific libraries.

---

7.17 The `csv` Module

Python includes a built-in module called `csv`.

We can import it using:

```python
import csv
```

Suppose `diode_data.csv` contains:

```text
Voltage,Current
0.1,0.001
0.2,0.002
0.3,0.004
0.4,0.008
```

We can read it using:

```python
import csv

with open("diode_data.csv", "r", newline="") as file:
    reader = csv.reader(file)

    for row in reader:
        print(row)
```

Output:

```text
['Voltage', 'Current']
['0.1', '0.001']
['0.2', '0.002']
['0.3', '0.004']
['0.4', '0.008']
```

Notice that the values read from a CSV file are initially strings.

---

7.18 Converting CSV Values to Numbers

The values in the CSV file are text when first read.

For example:

```text
'0.1'
```

is a string, not the numerical value:

```text
0.1
```

Therefore, numerical values must be converted before calculations.

For example:

```python
import csv

with open("diode_data.csv", "r", newline="") as file:
    reader = csv.reader(file)

    next(reader)

    for row in reader:
        V = float(row[0])
        I = float(row[1])

        print(V, I)
```

Output:

```text
0.1 0.001
0.2 0.002
0.3 0.004
0.4 0.008
```

The `next(reader)` statement skips the header row.

---

7.19 Reading CSV Data as Dictionaries

When a CSV file has column headings, `csv.DictReader` can make the data easier to understand.

```python
import csv

with open("diode_data.csv", "r", newline="") as file:
    reader = csv.DictReader(file)

    for row in reader:
        print(row)
```

A row may look like:

```text
{'Voltage': '0.1', 'Current': '0.001'}
```

We can access the values using column names:

```python
V = float(row["Voltage"])
I = float(row["Current"])
```

This is often clearer than using numerical column positions.

---

7.20 CSV Data and Scientific Calculations

Suppose the file contains measured voltage and current.

We can calculate resistance for each observation using:

$$
R=\frac{V}{I}.
$$

Python:

```python
import csv

with open("diode_data.csv", "r", newline="") as file:
    reader = csv.DictReader(file)

    for row in reader:
        V = float(row["Voltage"])
        I = float(row["Current"])

        R = V / I

        print(f"V = {V:.2f} V, I = {I:.4f} A, R = {R:.2f} ohm")
```

The program has moved from:

```text
CSV file
    ↓
Read data
    ↓
Convert values
    ↓
Apply equation
    ↓
Display result
```

This is the beginning of real scientific data processing.

---

7.21 Writing CSV Files

Python can also create CSV files.

Suppose we calculate current for several voltages.

```python
import csv

voltages = [1, 2, 3, 4, 5]
R = 100

with open("current_results.csv", "w", newline="") as file:
    writer = csv.writer(file)

    writer.writerow(["Voltage", "Current"])

    for V in voltages:
        I = V / R
        writer.writerow([V, I])
```

The resulting CSV file will contain approximately:

```text
Voltage,Current
1,0.01
2,0.02
3,0.03
4,0.04
5,0.05
```

This file can be opened in a spreadsheet or read by another Python program.

---

7.22 Why Saving Results Matters

Suppose an experiment is analyzed today.

If the calculated results are only printed on the screen, they may be lost when the program ends.

Saving results to a file allows:

- later inspection;
- comparison with another experiment;
- plotting at a later time;
- sharing with another researcher;
- reproducibility;
- further analysis.

A scientific workflow should therefore preserve important inputs and outputs.

---

7.23 Reading and Writing as a Scientific Workflow

A simple analysis can be represented as:

```text
Input data
    ↓
Read file
    ↓
Convert data
    ↓
Check data
    ↓
Calculate
    ↓
Save results
```

For example:

```text
diode_data.csv
       ↓
     Python
       ↓
   Calculate R
       ↓
current_results.csv
```

This is a basic form of a reproducible data pipeline.

---

7.24 File Handling and Reproducibility

Suppose a student reports:

> "The calculated resistance was 220 ohm."

A more reproducible analysis should preserve:

- the original measurement file;
- the Python code;
- the calculation;
- the resulting output file.

Then another person can inspect how the result was produced.

A reproducible workflow can be represented as:

$$
\boxed{
\text{Data}
+
\text{Code}
+
\text{Method}
\rightarrow
\text{Result}
}
$$

This idea will become increasingly important as the course moves toward scientific data analysis.

---

7.25 Common Beginner Mistakes

### Mistake 1 — Forgetting the file path

Python looks for a file relative to the current working directory unless another path is specified.

### Mistake 2 — Using `"w"` accidentally

Writing with `"w"` can replace an existing file.

Use `"a"` when you intentionally want to append.

### Mistake 3 — Forgetting that CSV values are strings

For example:

```python
V = row["Voltage"]
```

does not automatically create a floating-point number.

Use:

```python
V = float(row["Voltage"])
```

when numerical calculation is required.

### Mistake 4 — Forgetting the header

If the CSV contains column headings, make sure the header is handled appropriately.

### Mistake 5 — Changing the original experimental data

The original data file should normally be preserved.

Perform cleaning or transformation on a copy or save processed data separately.

### Mistake 6 — Saving only the final result

For reproducibility, preserve the original input data and the code used to generate the result.

---

7.26 Worked Example: Read a Diode CSV File

Suppose `diode_data.csv` contains:

```text
Voltage,Current
0.1,0.001
0.2,0.002
0.3,0.004
0.4,0.008
```

We want to calculate resistance for each measurement.

The mathematical relationship is:

$$
R=\frac{V}{I}.
$$

Python:

```python
import csv

with open("diode_data.csv", "r", newline="") as file:
    reader = csv.DictReader(file)

    for row in reader:
        V = float(row["Voltage"])
        I = float(row["Current"])

        R = V / I

        print(f"V = {V:.2f} V, I = {I:.4f} A, R = {R:.2f} ohm")
```

Output:

```text
V = 0.10 V, I = 0.0010 A, R = 100.00 ohm
V = 0.20 V, I = 0.0020 A, R = 100.00 ohm
V = 0.30 V, I = 0.0040 A, R = 75.00 ohm
V = 0.40 V, I = 0.0080 A, R = 50.00 ohm
```

The calculation reveals that the ratio \(V/I\) is not constant across the measurements.

This is an important scientific observation.

Python performs the calculation; the scientist must interpret why the ratio changes.

---

7.27 Worked Example: Save Calculated Results

Suppose the same diode data is used to calculate resistance and save the result.

```python
import csv

with open("diode_data.csv", "r", newline="") as input_file:
    reader = csv.DictReader(input_file)

    with open("resistance_results.csv", "w", newline="") as output_file:
        writer = csv.writer(output_file)

        writer.writerow(["Voltage", "Current", "Resistance"])

        for row in reader:
            V = float(row["Voltage"])
            I = float(row["Current"])

            R = V / I

            writer.writerow([V, I, R])
```

The resulting file contains:

```text
Voltage,Current,Resistance
0.1,0.001,100.0
0.2,0.002,100.0
0.3,0.004,75.0
0.4,0.008,50.0
```

We have created a simple analysis pipeline:

```text
diode_data.csv
      ↓
 Read measurements
      ↓
 Calculate resistance
      ↓
resistance_results.csv
```

---

7.28 Exercises

### Exercise 1 — Write a Text File

Create a Python program that writes the following information to `experiment.txt`:

```text
Experiment: Resistor Test
Voltage: 5 V
Resistance: 100 ohm
```

---

### Exercise 2 — Read a Text File

Read the file created in Exercise 1 and display its contents.

Use:

```python
with open(...)
```

---

### Exercise 3 — Append Results

Append the following line to the same file:

```text
Current: 0.05 A
```

Verify that the original contents remain.

---

### Exercise 4 — Read a CSV File

Create a CSV file containing:

```text
Voltage,Current
1,0.01
2,0.02
3,0.03
4,0.04
5,0.05
```

Write a Python program to read the file and display each measurement.

---

### Exercise 5 — Calculate Resistance

Read the CSV file from Exercise 4 and calculate:

$$
R=\frac{V}{I}
$$

for each observation.

---

### Exercise 6 — Save Results

Modify Exercise 5 so that the calculated resistance is saved to:

```text
resistance_results.csv
```

Use the columns:

```text
Voltage,Current,Resistance
```

---

### Exercise 7 — Semiconductor Data

Create a CSV file containing voltage and current measurements for a hypothetical diode.

Read the file and calculate the ratio:

$$
\frac{V}{I}
$$

for each observation.

---

7.29 Think and Apply

A student receives an experimental CSV file and immediately starts modifying the original file to remove values that appear unusual.

Why might this be a poor scientific practice?

Think about:

- preserving original data;
- reproducibility;
- data validation;
- traceability;
- comparing original and processed datasets.

---

7.30 Chapter Summary

In this chapter, we learned that:

1. Scientific programs often need to read data from files and save results.
2. Files provide persistent storage.
3. `open()` is used to access files.
4. Common file modes include `"r"`, `"w"`, and `"a"`.
5. The `with open(...)` pattern provides safe and convenient file handling.
6. Text files can store readable scientific information.
7. CSV files are useful for tabular experimental data.
8. Python's built-in `csv` module can read and write CSV files.
9. Values read from CSV files are initially strings and may need conversion.
10. `csv.DictReader` provides convenient access to data using column names.
11. Results can be saved to new files for later analysis.
12. Preserving input data and analysis results supports reproducibility.
13. File handling forms the bridge between Python programming and real experimental datasets.

---

7.31 Key Takeaway

> **Scientific Python becomes much more useful when it can work with data outside the program itself. File handling connects experimental instruments and stored datasets to Python-based analysis.**

The next chapter begins the scientific computing part of Unit I with **NumPy arrays**, which provide efficient numerical structures for working with collections of scientific measurements.
