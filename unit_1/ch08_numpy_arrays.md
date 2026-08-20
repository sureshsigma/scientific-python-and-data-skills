# Chapter 8 — NumPy Arrays

## 8.1 Learning Objectives

After completing this chapter, you should be able to:

- explain why NumPy is important for scientific Python;
- understand the difference between a Python list and a NumPy array;
- import NumPy and create arrays;
- inspect the shape, size, and data type of an array;
- access elements using indexing;
- use slicing with NumPy arrays;
- perform arithmetic operations on arrays;
- understand element-wise operations;
- create arrays using useful NumPy functions;
- apply NumPy arrays to simple circuit and semiconductor datasets.

---

8.2 Why Do We Need NumPy?

Python lists are useful for storing collections of values.

For example:

```python
voltages = [1, 2, 3, 4, 5]
```

However, scientific computing often requires operations on large collections of numerical values.

Suppose an experiment produces:

$$
10,000
$$

voltage measurements.

We may want to:

- add or subtract values;
- multiply all measurements by a constant;
- calculate mathematical functions;
- calculate statistical quantities;
- create evenly spaced numerical values;
- perform matrix and vector operations.

Python lists can store these values, but they are not designed specifically for efficient numerical computation.

This is where **NumPy** becomes important.

NumPy stands for:

> Numerical Python

It provides efficient numerical data structures and mathematical operations for scientific computing.

---

8.3 Importing NumPy

NumPy is normally imported using:

```python
import numpy as np
```

The abbreviation `np` is a widely used convention.

After importing NumPy, we can use:

```python
np
```

to access its functions.

For example:

```python
import numpy as np

x = np.array([1, 2, 3])

print(x)
```

Output:

```text
[1 2 3]
```

The object `x` is a NumPy array.

---

8.4 Creating a NumPy Array

The main function used to create an array from existing values is:

```python
np.array()
```

For example:

```python
import numpy as np

voltage = np.array([1, 2, 3, 4, 5])

print(voltage)
```

Output:

```text
[1 2 3 4 5]
```

We can inspect the type:

```python
print(type(voltage))
```

Output:

```text
<class 'numpy.ndarray'>
```

`ndarray` means **N-dimensional array**.

---

8.5 NumPy Array versus Python List

Consider:

```python
values = [1, 2, 3, 4, 5]
```

This is a Python list.

Now:

```python
values = np.array([1, 2, 3, 4, 5])
```

is a NumPy array.

Both store multiple values, but they behave differently for numerical operations.

For example:

```python
values = [1, 2, 3]

print(values * 2)
```

Output:

```text
[1, 2, 3, 1, 2, 3]
```

List multiplication repeats the list.

Now consider:

```python
values = np.array([1, 2, 3])

print(values * 2)
```

Output:

```text
[2 4 6]
```

The NumPy array performs element-wise multiplication.

Mathematically:

$$
2
\begin{bmatrix}
1\\
2\\
3
\end{bmatrix}
=
\begin{bmatrix}
2\\
4\\
6
\end{bmatrix}.
$$

This is one of the major reasons NumPy is useful in scientific computing.

---

8.6 Element-Wise Operations

Suppose:

```python
V = np.array([1, 2, 3, 4, 5])
```

We can multiply every value by 2:

```python
print(V * 2)
```

Output:

```text
[ 2  4  6  8 10]
```

Add 1:

```python
print(V + 1)
```

Output:

```text
[2 3 4 5 6]
```

Subtract 1:

```python
print(V - 1)
```

Output:

```text
[0 1 2 3 4]
```

Divide by 2:

```python
print(V / 2)
```

Output:

```text
[0.5 1.  1.5 2.  2.5]
```

The operation is applied to every element.

---

8.7 Mathematical Interpretation

If

$$
\mathbf{V}
=
\begin{bmatrix}
V_1\\
V_2\\
V_3\\
V_4\\
V_5
\end{bmatrix},
$$

then:

```python
V * 2
```

performs:

$$
2\mathbf{V}
=
\begin{bmatrix}
2V_1\\
2V_2\\
2V_3\\
2V_4\\
2V_5
\end{bmatrix}.
$$

This is the basic idea of **vectorized computation**.

Instead of writing a loop explicitly, NumPy applies the operation efficiently across the array.

---

8.8 Adding Two Arrays

Consider:

```python
V1 = np.array([1, 2, 3])
V2 = np.array([4, 5, 6])
```

We can add them:

```python
print(V1 + V2)
```

Output:

```text
[5 7 9]
```

The operation is element-wise:

$$
\begin{bmatrix}
1\\
2\\
3
\end{bmatrix}
+
\begin{bmatrix}
4\\
5\\
6
\end{bmatrix}
=
\begin{bmatrix}
5\\
7\\
9
\end{bmatrix}.
$$

Similarly:

```python
print(V2 - V1)
```

Output:

```text
[3 3 3]
```

---

8.9 Multiplication of Two Arrays

Consider:

```python
V = np.array([1, 2, 3])
I = np.array([2, 4, 6])
```

We can calculate:

```python
P = V * I

print(P)
```

Output:

```text
[ 2  8 18]
```

This represents the element-wise calculation:

$$
P_i=V_iI_i.
$$

Therefore:

$$
\begin{aligned}
P_1 &= 1\times2=2,\\
P_2 &= 2\times4=8,\\
P_3 &= 3\times6=18.
\end{aligned}
$$

This is particularly useful when working with paired experimental measurements.

---

8.10 Division of Arrays

Similarly:

```python
V = np.array([1, 2, 3])
I = np.array([0.01, 0.02, 0.03])

R = V / I

print(R)
```

Output:

```text
[100. 100. 100.]
```

The operation represents:

$$
R_i=\frac{V_i}{I_i}.
$$

This allows a calculation to be performed across an entire collection of measurements.

---

8.11 Array Data Types

A NumPy array has a data type.

For example:

```python
x = np.array([1, 2, 3])

print(x.dtype)
```

Output may be:

```text
int64
```

For floating-point values:

```python
x = np.array([1.0, 2.0, 3.0])

print(x.dtype)
```

Output may be:

```text
float64
```

The exact integer representation can depend on the system.

The important idea is that NumPy arrays have a defined numerical data type.

---

8.12 Creating Floating-Point Arrays

Scientific measurements are often represented using floating-point values.

For example:

```python
voltage = np.array([0.1, 0.2, 0.3, 0.4])
```

Check:

```python
print(voltage.dtype)
```

Output will typically indicate a floating-point type.

We can also explicitly specify the data type:

```python
voltage = np.array([1, 2, 3], dtype=float)

print(voltage)
print(voltage.dtype)
```

Output:

```text
[1. 2. 3.]
float64
```

Explicit data types can be useful when numerical precision and consistency matter.

---

8.13 Array Indexing

NumPy arrays use zero-based indexing, just like Python lists.

Consider:

```python
V = np.array([1, 2, 3, 4, 5])
```

Then:

```python
print(V[0])
```

Output:

```text
1
```

and:

```python
print(V[3])
```

Output:

```text
4
```

The first element is at index `0`.

---

8.14 Negative Indexing

NumPy arrays also support negative indexing.

```python
V = np.array([1, 2, 3, 4, 5])

print(V[-1])
```

Output:

```text
5
```

The index `-1` refers to the last element.

---

8.15 Array Slicing

NumPy arrays support slicing.

```python
V = np.array([1, 2, 3, 4, 5])

print(V[1:4])
```

Output:

```text
[2 3 4]
```

The general form is:

```python
array[start:stop]
```

The stop index is not included.

We can also use a step:

```python
print(V[0:5:2])
```

Output:

```text
[1 3 5]
```

---

8.16 Changing Array Elements

NumPy arrays can be modified.

```python
V = np.array([1, 2, 3, 4, 5])

V[2] = 3.5

print(V)
```

Output:

```text
[1.  2.  3.5 4.  5. ]
```

Notice that the array may become floating-point because the value `3.5` cannot be represented as an integer.

This is an important reminder that array data types influence what values can be stored.

---

8.17 Array Shape

NumPy arrays can have one or more dimensions.

For a one-dimensional array:

```python
V = np.array([1, 2, 3, 4, 5])

print(V.shape)
```

Output:

```text
(5,)
```

This means the array has five elements along one dimension.

The shape describes the structure of the array.

---

8.18 Array Size

The `size` attribute gives the total number of elements.

```python
V = np.array([1, 2, 3, 4, 5])

print(V.size)
```

Output:

```text
5
```

For a one-dimensional array:

$$
\text{size}=\text{number of elements}.
$$

---

8.19 Number of Dimensions

The `ndim` attribute tells us how many dimensions the array has.

```python
V = np.array([1, 2, 3, 4, 5])

print(V.ndim)
```

Output:

```text
1
```

A one-dimensional array has:

$$
\text{ndim}=1.
$$

Later, two-dimensional arrays will be useful for tables and matrices.

---

8.20 Creating Arrays with `arange()`

NumPy provides `np.arange()` for generating sequences of numerical values.

For example:

```python
x = np.arange(0, 10, 2)

print(x)
```

Output:

```text
[0 2 4 6 8]
```

The structure is:

```python
np.arange(start, stop, step)
```

The stop value is generally excluded.

This is similar to Python's `range()` but produces a NumPy array and supports numerical steps.

---

8.21 `arange()` with Floating-Point Steps

We can write:

```python
x = np.arange(0, 1, 0.2)

print(x)
```

Output may be approximately:

```text
[0.  0.2 0.4 0.6 0.8]
```

Because floating-point numbers are represented with finite precision, `arange()` should be used carefully when an exact number of points is required.

For that purpose, `np.linspace()` is often preferable.

---

8.22 Creating Evenly Spaced Values with `linspace()`

NumPy provides:

```python
np.linspace()
```

for generating a specified number of evenly spaced values.

For example:

```python
x = np.linspace(0, 10, 6)

print(x)
```

Output:

```text
[ 0.  2.  4.  6.  8. 10.]
```

The function:

```python
np.linspace(start, stop, number_of_points)
```

includes both the start and stop values by default.

---

8.23 Why `linspace()` Is Important in Scientific Computing

Suppose we want to evaluate a mathematical model over:

$$
0\leq x\leq1.
$$

We want 11 evenly spaced points.

```python
x = np.linspace(0, 1, 11)

print(x)
```

Output:

```text
[0.  0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1. ]
```

The spacing is:

$$
\Delta x=\frac{x_{\max}-x_{\min}}{N-1}.
$$

For this example:

$$
\Delta x
=
\frac{1-0}{11-1}
=
0.1.
$$

This is extremely useful when generating values for mathematical models and scientific plots.

---

8.24 `arange()` versus `linspace()`

| Feature | `np.arange()` | `np.linspace()` |
| --- | --- | --- |
| Main idea | Specify step | Specify number of points |
| Typical form | `start, stop, step` | `start, stop, number` |
| Stop included by default | No | Yes |
| Useful for | Step-based sequences | Controlled sampling |
| Floating-point use | Requires care | Often preferable |

A useful rule is:

> Use `arange()` when the step size is the main requirement.

> Use `linspace()` when the number of points is the main requirement.

This distinction will become important when we create scientific graphs.

---

8.25 Creating Arrays of Zeros

NumPy provides:

```python
np.zeros()
```

For example:

```python
x = np.zeros(5)

print(x)
```

Output:

```text
[0. 0. 0. 0. 0.]
```

This creates an array containing five zeros.

Such arrays can be useful for initializing storage for calculations.

---

8.26 Creating Arrays of Ones

Similarly:

```python
x = np.ones(5)

print(x)
```

Output:

```text
[1. 1. 1. 1. 1.]
```

The values are floating-point by default.

These functions are often used when preparing numerical calculations.

---

8.27 Two-Dimensional Arrays

NumPy also supports two-dimensional arrays.

For example:

```python
data = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

print(data)
```

Output:

```text
[[1 2 3]
 [4 5 6]]
```

This can be interpreted as a table:

$$
\begin{bmatrix}
1 & 2 & 3\\
4 & 5 & 6
\end{bmatrix}.
$$

The array has:

```python
print(data.shape)
```

Output:

```text
(2, 3)
```

This means:

- 2 rows;
- 3 columns.

---

8.28 Accessing Elements in a Two-Dimensional Array

Consider:

```python
data = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
```

We can access the first row:

```python
print(data[0])
```

Output:

```text
[1 2 3]
```

We can access the element in row 2, column 3:

```python
print(data[1, 2])
```

Output:

```text
6
```

The indexing is:

```text
data[row, column]
```

Both indices start at zero.

---

8.29 A Scientific Data Table

Suppose an experiment records:

| Voltage | Current | Temperature |
| ---: | ---: | ---: |
| 0.1 | 0.001 | 300 |
| 0.2 | 0.002 | 300 |
| 0.3 | 0.004 | 300 |
| 0.4 | 0.008 | 300 |

We can represent the numerical part as:

```python
data = np.array([
    [0.1, 0.001, 300],
    [0.2, 0.002, 300],
    [0.3, 0.004, 300],
    [0.4, 0.008, 300]
])
```

The shape is:

```python
print(data.shape)
```

Output:

```text
(4, 3)
```

There are four observations and three variables.

---

8.30 Selecting a Column

For the data:

```python
data = np.array([
    [0.1, 0.001, 300],
    [0.2, 0.002, 300],
    [0.3, 0.004, 300],
    [0.4, 0.008, 300]
])
```

The voltage column is:

```python
voltage = data[:, 0]
```

The current column is:

```python
current = data[:, 1]
```

The temperature column is:

```python
temperature = data[:, 2]
```

Here:

```text
:
```

means all rows.

Therefore:

```python
data[:, 0]
```

means:

> all rows, column 0.

---

8.31 Selecting Rows

We can select a row using:

```python
print(data[0])
```

Output:

```text
[1.e-01 1.e-03 3.e+02]
```

The first row represents the first observation.

We can select multiple rows:

```python
print(data[0:2])
```

Output:

```text
[[1.e-01 1.e-03 3.e+02]
 [2.e-01 2.e-03 3.e+02]]
```

This allows subsets of measurements to be selected.

---

8.32 Vectorized Ohm's Law

Suppose:

```python
V = np.array([1, 2, 3, 4, 5])
R = 100
```

We can calculate all currents in one expression:

```python
I = V / R

print(I)
```

Output:

```text
[0.01 0.02 0.03 0.04 0.05]
```

Mathematically:

$$
\mathbf{I}=\frac{\mathbf{V}}{R}.
$$

This is a simple example of vectorized scientific computation.

No explicit `for` loop is required.

---

8.33 Vectorized Power Calculation

Suppose:

```python
V = np.array([1, 2, 3, 4, 5])
I = np.array([0.01, 0.02, 0.03, 0.04, 0.05])
```

We can calculate power:

```python
P = V * I

print(P)
```

Output:

```text
[0.01 0.04 0.09 0.16 0.25]
```

The mathematical relationship is:

$$
P_i=V_iI_i.
$$

The calculation is performed across the complete arrays.

---

8.34 NumPy and Experimental Data

Consider a diode experiment.

```python
V = np.array([0.1, 0.2, 0.3, 0.4, 0.5])
I = np.array([0.001, 0.002, 0.004, 0.008, 0.015])
```

We can calculate:

```python
R = V / I
```

Then:

```python
print(R)
```

Output:

```text
[100.          100.           75.           50.
  33.33333333]
```

The calculation:

$$
R=\frac{V}{I}
$$

has been applied to every measurement.

This is the beginning of array-based experimental data analysis.

---

8.35 NumPy Attributes to Remember

For a NumPy array `x`, the following attributes are especially useful:

```python
x.shape
x.size
x.ndim
x.dtype
```

They answer:

| Expression | Question |
| --- | --- |
| `x.shape` | What is the structure? |
| `x.size` | How many elements? |
| `x.ndim` | How many dimensions? |
| `x.dtype` | What numerical type? |

Example:

```python
x = np.array([1, 2, 3, 4])

print(x.shape)
print(x.size)
print(x.ndim)
print(x.dtype)
```

Output may be:

```text
(4,)
4
1
int64
```

---

8.36 Common Beginner Mistakes

### Mistake 1 — Forgetting to import NumPy

This will not work:

```python
x = np.array([1, 2, 3])
```

unless NumPy has been imported.

Use:

```python
import numpy as np
```

### Mistake 2 — Confusing lists and arrays

For:

```python
x = [1, 2, 3]
```

the expression:

```python
x * 2
```

repeats the list.

For:

```python
x = np.array([1, 2, 3])
```

the expression:

```python
x * 2
```

multiplies every element.

### Mistake 3 — Forgetting zero-based indexing

The first element is:

```python
x[0]
```

not:

```python
x[1]
```

### Mistake 4 — Confusing `shape` and `size`

For:

```python
x = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
```

we have:

```text
shape = (2, 3)
size  = 6
```

### Mistake 5 — Assuming `arange()` and `linspace()` do the same thing

They solve related but different problems.

`arange()` emphasizes step size.

`linspace()` emphasizes the number of points.

---

8.37 Worked Example: Generate Voltage Values

Generate 11 evenly spaced voltage values from:

$$
0\,\mathrm{V}
$$

to

$$
5\,\mathrm{V}.
$$

Python:

```python
import numpy as np

V = np.linspace(0, 5, 11)

print(V)
```

Output:

```text
[0.  0.5 1.  1.5 2.  2.5 3.  3.5 4.  4.5 5. ]
```

The spacing is:

$$
\Delta V
=
\frac{5-0}{11-1}
=
0.5\,\mathrm{V}.
$$

---

8.38 Worked Example: Generate an Ohm's Law Dataset

Suppose:

$$
R=100\,\Omega.
$$

Generate 11 voltage values between 0 and 5 V and calculate current.

```python
import numpy as np

V = np.linspace(0, 5, 11)
R = 100

I = V / R

print(V)
print(I)
```

The arrays represent:

$$
\mathbf{V}
=
[0,0.5,1,\ldots,5],
$$

and:

$$
\mathbf{I}
=
\frac{\mathbf{V}}{100}.
$$

This dataset can later be visualized as an I-V relationship.

---

8.39 Worked Example: Create a Simple Measurement Table

```python
import numpy as np

V = np.array([0.1, 0.2, 0.3, 0.4, 0.5])
I = np.array([0.001, 0.002, 0.004, 0.008, 0.015])

R = V / I

print("Voltage (V):", V)
print("Current (A):", I)
print("V/I (ohm):", R)
```

Output:

```text
Voltage (V): [0.1 0.2 0.3 0.4 0.5]
Current (A): [0.001 0.002 0.004 0.008 0.015]
V/I (ohm): [100.         100.          75.          50.
  33.33333333]
```

The calculation demonstrates how NumPy can operate directly on collections of measurements.

---

8.40 Exercises

### Exercise 1 — Create an Array

Create a NumPy array containing:

$$
10,20,30,40,50.
$$

Display:

- the array;
- its size;
- its number of dimensions;
- its data type.

---

### Exercise 2 — Array Arithmetic

For:

```python
x = np.array([1, 2, 3, 4, 5])
```

calculate:

```text
x + 10
x - 1
x * 3
x / 2
```

---

### Exercise 3 — Voltage and Current

Create:

```python
V = np.array([1, 2, 3, 4, 5])
```

and calculate current for:

$$
R=100\,\Omega.
$$

---

### Exercise 4 — `arange()`

Use `np.arange()` to create:

```text
0, 2, 4, 6, 8, 10
```

---

### Exercise 5 — `linspace()`

Use `np.linspace()` to create 11 equally spaced values between:

$$
0
$$

and

$$
1.
$$

Verify the spacing.

---

### Exercise 6 — Two-Dimensional Array

Create the array:

$$
\begin{bmatrix}
1&2&3\\
4&5&6
\end{bmatrix}.
$$

Display:

- its shape;
- its size;
- its first row;
- its second column.

---

### Exercise 7 — Semiconductor Data

Create two NumPy arrays:

```python
V = [0.1, 0.2, 0.3, 0.4, 0.5]
I = [0.001, 0.002, 0.004, 0.008, 0.015]
```

Calculate:

$$
R=\frac{V}{I}.
$$

Display the result.

---

8.41 Think and Apply

Suppose an experiment produces 10,000 voltage measurements.

Compare these two approaches:

```python
voltages = [...]
```

and:

```python
voltages = np.array([...])
```

Why is the NumPy array more appropriate for numerical scientific analysis?

Consider:

- mathematical operations;
- vectorized computation;
- numerical functions;
- array structure;
- later visualization and statistical analysis.

---

8.42 Chapter Summary

In this chapter, we learned that:

1. NumPy is a fundamental library for scientific Python.
2. NumPy provides the `ndarray` structure for numerical data.
3. Arrays support efficient element-wise mathematical operations.
4. NumPy arrays have attributes such as `shape`, `size`, `ndim`, and `dtype`.
5. Array indexing starts at zero.
6. Arrays support slicing.
7. `np.arange()` generates values using a step size.
8. `np.linspace()` generates a specified number of evenly spaced values.
9. `np.zeros()` and `np.ones()` can initialize numerical arrays.
10. NumPy supports multidimensional arrays.
11. Vectorized operations allow calculations to be applied to complete arrays without explicitly writing a loop.
12. NumPy arrays provide an important foundation for scientific data analysis.
13. Circuit and semiconductor measurements can be represented naturally as numerical arrays.

---

8.43 Key Takeaway

> **NumPy changes Python from a general-purpose programming language into a powerful numerical computing environment. Arrays allow collections of scientific measurements to be manipulated mathematically as a single object.**

The next chapter develops this foundation further by focusing on **NumPy array operations and mathematical calculations**, including arithmetic, mathematical functions, and numerical transformations used in scientific and semiconductor applications.
