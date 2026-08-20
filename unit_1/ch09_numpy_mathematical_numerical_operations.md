# Chapter 9 — NumPy Mathematical and Numerical Operations

## 9.1 Learning Objectives

After completing this chapter, you should be able to:

- perform arithmetic operations on NumPy arrays;
- apply mathematical functions to complete arrays;
- understand element-wise mathematical operations;
- use powers, roots, exponentials, and logarithms;
- work with trigonometric functions;
- calculate absolute values and signs;
- perform basic numerical transformations;
- use NumPy operations for circuit and semiconductor calculations;
- distinguish between scalar and array calculations;
- interpret NumPy calculations using mathematical notation.

---

9.2 From Arrays to Scientific Computation

In the previous chapter, we created NumPy arrays.

For example:

```python
import numpy as np

V = np.array([1, 2, 3, 4, 5])
```

The real power of NumPy appears when mathematical operations are applied directly to these arrays.

Suppose:

$$
V=
\begin{bmatrix}
1\\
2\\
3\\
4\\
5
\end{bmatrix}.
$$

We can calculate:

$$
2V,
$$

$$
V^2,
$$

$$
\sqrt{V},
$$

$$
e^V,
$$

and many other mathematical transformations.

NumPy provides functions that perform these operations efficiently.

The general idea is:

```text
Numerical data
      ↓
NumPy array
      ↓
Mathematical operation
      ↓
Numerical result
```

---

9.3 Scalar versus Array Operations

A **scalar** is a single numerical value.

For example:

```python
V = 5
```

An **array** contains multiple values:

```python
V = np.array([1, 2, 3, 4, 5])
```

The same mathematical operation can be applied to both.

For a scalar:

```python
V = 5

print(V * 2)
```

Output:

```text
10
```

For an array:

```python
V = np.array([1, 2, 3, 4, 5])

print(V * 2)
```

Output:

```text
[ 2  4  6  8 10]
```

The array operation is equivalent to applying the calculation to every element.

---

9.4 Addition and Subtraction

Consider:

```python
x = np.array([1, 2, 3, 4])
```

Add 10:

```python
print(x + 10)
```

Output:

```text
[11 12 13 14]
```

Subtract 1:

```python
print(x - 1)
```

Output:

```text
[0 1 2 3]
```

Mathematically:

$$
x_i+10
$$

and:

$$
x_i-1
$$

are applied to every element.

---

9.5 Multiplication and Division

For:

```python
x = np.array([1, 2, 3, 4])
```

we can calculate:

```python
print(x * 3)
```

Output:

```text
[ 3  6  9 12]
```

and:

```python
print(x / 2)
```

Output:

```text
[0.5 1.  1.5 2. ]
```

These operations are fundamental when converting or scaling scientific measurements.

---

9.6 Multiplying an Array by a Physical Constant

Suppose voltage is measured in millivolts:

```python
V_mV = np.array([100, 200, 300, 400])
```

To convert millivolts to volts:

$$
V_{\mathrm{V}}
=
V_{\mathrm{mV}}\times10^{-3}.
$$

Python:

```python
V_V = V_mV * 1e-3

print(V_V)
```

Output:

```text
[0.1 0.2 0.3 0.4]
```

This is a simple example of unit conversion using vectorized computation.

---

9.7 Multiplication of Two Arrays

Suppose:

```python
V = np.array([1, 2, 3])
I = np.array([0.1, 0.2, 0.3])
```

Electrical power is:

$$
P=VI.
$$

Python:

```python
P = V * I

print(P)
```

Output:

```text
[0.1 0.4 0.9]
```

The multiplication is element-wise:

$$
P_i=V_iI_i.
$$

Therefore:

$$
P=
\begin{bmatrix}
0.1\\
0.4\\
0.9
\end{bmatrix}.
$$

---

9.8 Division of Two Arrays

Suppose:

```python
V = np.array([1, 2, 3])
I = np.array([0.01, 0.02, 0.03])
```

Resistance can be calculated as:

$$
R=\frac{V}{I}.
$$

Python:

```python
R = V / I

print(R)
```

Output:

```text
[100. 100. 100.]
```

NumPy applies the division element by element.

---

9.9 Powers

The `**` operator can be used for powers.

For example:

```python
x = np.array([1, 2, 3, 4])

print(x ** 2)
```

Output:

```text
[ 1  4  9 16]
```

Mathematically:

$$
x^2
$$

is applied to every element.

Similarly:

```python
print(x ** 3)
```

produces:

```text
[ 1  8 27 64]
```

---

9.10 Square Roots

NumPy provides:

```python
np.sqrt()
```

For example:

```python
x = np.array([1, 4, 9, 16])

print(np.sqrt(x))
```

Output:

```text
[1. 2. 3. 4.]
```

Mathematically:

$$
\sqrt{
\begin{bmatrix}
1\\4\\9\\16
\end{bmatrix}
}
=
\begin{bmatrix}
1\\2\\3\\4
\end{bmatrix}.
$$

Square roots appear in many scientific equations.

---

9.11 Exponential Function

The exponential function is:

$$
e^x.
$$

NumPy provides:

```python
np.exp()
```

For example:

```python
x = np.array([0, 1, 2])

print(np.exp(x))
```

Output is approximately:

```text
[1.         2.71828183 7.3890561 ]
```

The values correspond to:

$$
e^0=1,
$$

$$
e^1\approx2.718,
$$

$$
e^2\approx7.389.
$$

Exponential functions are particularly important in semiconductor physics.

---

9.12 Exponential Growth

Consider the mathematical model:

$$
y=e^x.
$$

We can create values:

```python
x = np.array([0, 1, 2, 3, 4])

y = np.exp(x)

print(y)
```

The result contains the exponential value for each element of `x`.

Later, this type of calculation will help us understand models such as diode current.

---

9.13 Natural Logarithm

The natural logarithm is:

$$
\ln(x).
$$

NumPy provides:

```python
np.log()
```

For example:

```python
x = np.array([1, np.e, np.e**2])

print(np.log(x))
```

Output is approximately:

```text
[0. 1. 2.]
```

because:

$$
\ln(1)=0,
$$

$$
\ln(e)=1,
$$

and:

$$
\ln(e^2)=2.
$$

---

9.14 Logarithm with Base 10

For the base-10 logarithm:

$$
\log_{10}(x),
$$

use:

```python
np.log10()
```

Example:

```python
x = np.array([1, 10, 100, 1000])

print(np.log10(x))
```

Output:

```text
[0. 1. 2. 3.]
```

This is useful when working with quantities expressed on logarithmic scales.

---

9.15 Why Logarithms Matter in Semiconductor Science

Semiconductor relationships can span several orders of magnitude.

For example, current may vary from:

$$
10^{-12}\,\mathrm{A}
$$

to:

$$
10^{-3}\,\mathrm{A}.
$$

A logarithmic transformation makes such ranges easier to analyze.

For example:

```python
I = np.array([1e-12, 1e-9, 1e-6, 1e-3])

log_I = np.log10(I)

print(log_I)
```

Output:

```text
[-12.  -9.  -6.  -3.]
```

The logarithmic representation makes the order of magnitude explicit.

---

9.16 Absolute Value

NumPy provides:

```python
np.abs()
```

For example:

```python
x = np.array([-5, -2, 0, 3, 7])

print(np.abs(x))
```

Output:

```text
[5 2 0 3 7]
```

Mathematically:

$$
|x|
$$

is the distance of \(x\) from zero.

Absolute values are frequently used in error calculations.

---

9.17 Error Calculation

Suppose an experimental value is:

$$
x_{\mathrm{measured}}=10.2
$$

and the reference value is:

$$
x_{\mathrm{reference}}=10.0.
$$

The absolute error is:

$$
E_{\mathrm{abs}}
=
|x_{\mathrm{measured}}-x_{\mathrm{reference}}|.
$$

Python:

```python
measured = 10.2
reference = 10.0

error = np.abs(measured - reference)

print(error)
```

Output:

```text
0.1999999999999993
```

The small representation difference is due to floating-point arithmetic.

For practical reporting, we can format the result:

```python
print(f"Absolute error = {error:.2f}")
```

Output:

```text
Absolute error = 0.20
```

---

9.18 Percentage Error

Percentage error can be calculated as:

$$
\text{Percentage Error}
=
\frac{|x_{\mathrm{measured}}-x_{\mathrm{reference}}|}
{|x_{\mathrm{reference}}|}
\times100.
$$

Python:

```python
measured = 10.2
reference = 10.0

error_percent = (
    np.abs(measured - reference)
    / np.abs(reference)
    * 100
)

print(f"Percentage error = {error_percent:.2f}%")
```

Output:

```text
Percentage error = 2.00%
```

This calculation will become useful when we study experimental error analysis.

---

9.19 Trigonometric Functions

NumPy provides common trigonometric functions:

```python
np.sin()
np.cos()
np.tan()
```

These functions use angles measured in **radians**.

For example:

```python
x = np.array([0, np.pi / 2, np.pi])

print(np.sin(x))
```

Output is approximately:

```text
[0.0000000e+00 1.0000000e+00 1.2246468e-16]
```

The last value is theoretically:

$$
\sin(\pi)=0.
$$

The tiny numerical value results from floating-point computation.

---

9.20 Degrees and Radians

The relationship between degrees and radians is:

$$
180^\circ=\pi\ \mathrm{rad}.
$$

For example:

$$
90^\circ=\frac{\pi}{2}\,\mathrm{rad}.
$$

NumPy provides conversion functions:

```python
np.deg2rad()
np.rad2deg()
```

Example:

```python
angle_deg = np.array([0, 30, 60, 90])

angle_rad = np.deg2rad(angle_deg)

print(angle_rad)
```

Output:

```text
[0.         0.52359878 1.04719755 1.57079633]
```

---

9.21 Trigonometric Example

Suppose:

```python
angles_deg = np.array([0, 30, 60, 90])

angles_rad = np.deg2rad(angles_deg)

values = np.sin(angles_rad)

print(values)
```

Output is approximately:

```text
[0.        0.5       0.8660254 1.       ]
```

The corresponding mathematical values are:

$$
\sin(0^\circ)=0,
$$

$$
\sin(30^\circ)=0.5,
$$

$$
\sin(60^\circ)=\frac{\sqrt{3}}{2},
$$

$$
\sin(90^\circ)=1.
$$

---

9.22 Rounding Numerical Results

NumPy provides:

```python
np.round()
```

For example:

```python
x = np.array([1.23456, 2.34567, 3.45678])

print(np.round(x, 2))
```

Output:

```text
[1.23 2.35 3.46]
```

Rounding is useful for presentation.

However:

> Rounding a displayed result and changing the underlying data are not the same scientific operation.

During calculations, it is generally better to retain appropriate precision and round only when presenting results.

---

9.23 Floor and Ceiling

NumPy provides:

```python
np.floor()
np.ceil()
```

For example:

```python
x = np.array([1.2, 2.7, -1.2])

print(np.floor(x))
print(np.ceil(x))
```

Output:

```text
[ 1.  2. -2.]
[ 2.  3. -1.]
```

`floor()` moves toward negative infinity.

`ceil()` moves toward positive infinity.

These functions are useful in numerical algorithms, although they should not be confused with ordinary rounding.

---

9.24 Minimum and Maximum

NumPy provides:

```python
np.min()
np.max()
```

For example:

```python
V = np.array([0.5, 0.7, 0.6, 0.9, 0.8])

print(np.min(V))
print(np.max(V))
```

Output:

```text
0.5
0.9
```

These operations help identify the range of measured values.

The range is:

$$
R=x_{\max}-x_{\min}.
$$

For the voltage example:

```python
range_value = np.max(V) - np.min(V)

print(range_value)
```

Output:

```text
0.4
```

---

9.25 Basic Statistical Operations

NumPy also provides basic statistical functions.

For example:

```python
x = np.array([10, 20, 30, 40, 50])

print(np.mean(x))
```

Output:

```text
30.0
```

The arithmetic mean is:

$$
\bar{x}
=
\frac{1}{n}
\sum_{i=1}^{n}x_i.
$$

For the five values:

$$
\bar{x}
=
\frac{10+20+30+40+50}{5}
=
30.
$$

Statistical operations will be developed systematically in a later chapter.

---

9.26 NumPy and Semiconductor Equations

Many semiconductor equations contain mathematical functions such as:

- exponentials;
- logarithms;
- powers;
- square roots.

NumPy provides these operations directly.

For example, the idealized diode equation is:

$$
I
=
I_S
\left(
e^{\frac{V}{nV_T}}-1
\right).
$$

A simplified Python implementation can be written as:

```python
import numpy as np

Is = 1e-12
n = 1
Vt = 0.02585

V = np.array([0.1, 0.2, 0.3, 0.4, 0.5])

I = Is * (np.exp(V / (n * Vt)) - 1)

print(I)
```

The resulting currents vary rapidly with voltage.

This is a useful example because it connects:

```text
Physical model
      ↓
Mathematical equation
      ↓
NumPy function
      ↓
Array calculation
```

The detailed physical interpretation of the diode equation belongs to semiconductor/device courses. Here, the focus is on implementing the numerical expression.

---

9.27 Avoiding Numerical Overflow

Exponential functions can become extremely large.

For example:

```python
x = np.array([1, 10, 100, 1000])

print(np.exp(x))
```

Very large exponential arguments can produce extremely large values or numerical overflow.

This illustrates an important principle:

> Mathematical expressions that are valid on paper may require numerical care when implemented on a computer.

Numerical stability becomes increasingly important in scientific computing.

---

9.28 Combining Several Operations

Scientific calculations often involve multiple operations.

For example:

$$
y
=
\frac{\sqrt{x}+1}{x^2}.
$$

Python:

```python
x = np.array([1, 2, 3, 4])

y = (np.sqrt(x) + 1) / (x ** 2)

print(y)
```

Each operation is applied element-wise.

The expression follows the mathematical structure directly.

---

9.29 Parentheses and Order of Operations

Python follows standard mathematical operator precedence.

For example:

```python
y = a + b * c
```

means:

$$
y=a+bc,
$$

not:

$$
y=(a+b)c.
$$

Use parentheses when the mathematical structure needs to be explicit.

For example:

```python
y = (a + b) * c
```

Scientific code should prioritize clarity.

A slightly longer expression that clearly communicates the mathematics is often preferable to a compact but ambiguous expression.

---

9.30 Unit Conversion with NumPy

Suppose temperature measurements are given in Celsius:

```python
T_C = np.array([20, 25, 30, 35, 40])
```

Convert to Kelvin using:

$$
T_K=T_C+273.15.
$$

Python:

```python
T_K = T_C + 273.15

print(T_K)
```

Output:

```text
[293.15 298.15 303.15 308.15 313.15]
```

This is another example of a vectorized scientific transformation.

---

9.31 Converting Micrometres to Metres

Suppose a measurement is given in micrometres:

```python
length_um = np.array([1, 2, 5, 10])
```

Since:

$$
1\,\mu\mathrm{m}=10^{-6}\,\mathrm{m},
$$

we can write:

```python
length_m = length_um * 1e-6

print(length_m)
```

Output:

```text
[1.e-06 2.e-06 5.e-06 1.e-05]
```

Unit conversion is often one of the first numerical transformations applied to experimental data.

---

9.32 Working with Constants

Scientific calculations often involve constants.

For example:

```python
q = 1.602176634e-19
```

represents the elementary charge in coulombs.

We can use constants in array calculations.

For example, if an array represents the number of elementary charges:

```python
N = np.array([1, 10, 100])

q = 1.602176634e-19

Q = N * q

print(Q)
```

Output:

```text
[1.60217663e-19 1.60217663e-18 1.60217663e-17]
```

The important programming idea is that a scalar constant can be applied to an entire NumPy array.

---

9.33 Broadcasting: A First Look

Consider:

```python
V = np.array([1, 2, 3, 4])
R = 100
```

When we write:

```python
I = V / R
```

NumPy divides every element of `V` by the scalar `R`.

This behavior is part of NumPy's **broadcasting** mechanism.

Conceptually:

$$
\begin{bmatrix}
1\\
2\\
3\\
4
\end{bmatrix}
\div100
=
\begin{bmatrix}
0.01\\
0.02\\
0.03\\
0.04
\end{bmatrix}.
$$

We do not need to write a loop.

A detailed treatment of broadcasting will come later as needed.

---

9.34 Scientific Calculation with Multiple Arrays

Suppose:

```python
V = np.array([1, 2, 3, 4])
R = np.array([100, 200, 300, 400])
```

Then:

```python
I = V / R

print(I)
```

Output:

```text
[0.01 0.01 0.01 0.01]
```

Each voltage is divided by the corresponding resistance:

$$
I_i=\frac{V_i}{R_i}.
$$

This is useful when different measurements have different associated parameters.

---

9.35 Worked Example: Temperature-Dependent Resistance

Suppose a simple model is:

$$
R(T)=R_0[1+\alpha(T-T_0)].
$$

Let:

$$
R_0=100\,\Omega,
$$

$$
\alpha=0.004\,\mathrm{K}^{-1},
$$

and:

$$
T_0=300\,\mathrm{K}.
$$

We want to calculate resistance at:

$$
T=300,310,320,330\,\mathrm{K}.
$$

Python:

```python
import numpy as np

R0 = 100
alpha = 0.004
T0 = 300

T = np.array([300, 310, 320, 330])

R = R0 * (1 + alpha * (T - T0))

print(R)
```

Output:

```text
[100. 104. 108. 112.]
```

The equation has been applied to the entire temperature array.

---

9.36 Worked Example: Percentage Change

Suppose the initial value is:

$$
x_0=100.
$$

Measured values are:

```python
x = np.array([98, 101, 105, 110])
```

The percentage change is:

$$
\%\Delta x
=
\frac{x-x_0}{x_0}\times100.
$$

Python:

```python
x0 = 100
x = np.array([98, 101, 105, 110])

percentage_change = (x - x0) / x0 * 100

print(percentage_change)
```

Output:

```text
[-2.  1.  5. 10.]
```

The array calculation makes the transformation simple and transparent.

---

9.37 Worked Example: Semiconductor Exponential Model

Consider:

$$
y=Ae^{Bx}.
$$

Let:

$$
A=2,
\qquad
B=0.5.
$$

Create values of \(x\):

```python
import numpy as np

A = 2
B = 0.5

x = np.linspace(0, 5, 6)

y = A * np.exp(B * x)

print(x)
print(y)
```

The program evaluates the model for every value of \(x\).

This is a general pattern that will later be used for curve fitting and scientific visualization.

---

9.38 Common Beginner Mistakes

### Mistake 1 — Using a Python list when array arithmetic is required

For:

```python
x = [1, 2, 3]
```

the expression:

```python
x * 2
```

repeats the list.

Use:

```python
x = np.array([1, 2, 3])
```

when numerical element-wise multiplication is intended.

### Mistake 2 — Forgetting that trigonometric functions use radians

For example:

```python
np.sin(90)
```

does not mean \(\sin(90^\circ)\).

Convert degrees to radians first.

### Mistake 3 — Confusing `np.log()` and `np.log10()`

```python
np.log(x)
```

means the natural logarithm:

$$
\ln(x).
$$

```python
np.log10(x)
```

means:

$$
\log_{10}(x).
$$

### Mistake 4 — Rounding too early

Repeated rounding can introduce unnecessary numerical error.

Keep sufficient precision during calculations and round when presenting results.

### Mistake 5 — Ignoring units

Python does not automatically know whether:

```python
5
```

means volts, amperes, metres, or another unit.

The programmer must maintain unit consistency.

### Mistake 6 — Using mathematically unstable expressions

Very large exponentials or divisions by very small numbers can create numerical problems.

Scientific computing requires awareness of numerical limitations.

---

9.39 Exercises

### Exercise 1 — Array Arithmetic

Create:

```python
x = np.array([1, 2, 3, 4, 5])
```

Calculate:

$$
x^2,
$$

$$
2x+1,
$$

and:

$$
\sqrt{x}.
$$

---

### Exercise 2 — Unit Conversion

Create an array of voltages in millivolts:

```text
[100, 200, 500, 1000]
```

Convert them to volts.

---

### Exercise 3 — Logarithms

Create:

```python
x = np.array([1, 10, 100, 1000])
```

Calculate:

```python
np.log10(x)
```

---

### Exercise 4 — Trigonometry

Create angles:

```text
0°, 30°, 45°, 60°, 90°
```

Convert them to radians and calculate their sine values.

---

### Exercise 5 — Error

Measured values:

```python
measured = np.array([10.1, 9.8, 10.3, 10.0])
```

Reference value:

$$
x_0=10.
$$

Calculate the absolute error for each observation.

---

### Exercise 6 — Percentage Error

Using the same data, calculate:

$$
\frac{|x-x_0|}{|x_0|}\times100.
$$

---

### Exercise 7 — Semiconductor Exponential Model

Use:

$$
y=2e^{0.5x}
$$

for:

$$
0\leq x\leq5.
$$

Generate six equally spaced values of \(x\) and calculate \(y\).

---

### Exercise 8 — Temperature-Dependent Resistance

Use:

$$
R(T)=R_0[1+\alpha(T-T_0)]
$$

with:

$$
R_0=100\,\Omega,
\qquad
\alpha=0.004\,\mathrm{K}^{-1},
\qquad
T_0=300\,\mathrm{K}.
$$

Calculate resistance for:

```text
300 K, 310 K, 320 K, 330 K, 340 K
```

---

9.40 Think and Apply

Consider the following expression:

```python
I = Is * (np.exp(V / (n * Vt)) - 1)
```

1. Which part represents the exponential function?
2. Which values are scalars?
3. Which value can be an array?
4. What happens if `V` contains many voltage measurements?
5. Why is NumPy useful for implementing this model?

---

9.41 Chapter Summary

In this chapter, we learned that:

1. NumPy performs mathematical operations directly on arrays.
2. Scalar values can be combined with arrays through broadcasting.
3. Array arithmetic is generally element-wise.
4. Powers can be calculated using `**`.
5. `np.sqrt()` calculates square roots.
6. `np.exp()` calculates exponential values.
7. `np.log()` calculates natural logarithms.
8. `np.log10()` calculates base-10 logarithms.
9. `np.abs()` calculates absolute values.
10. NumPy provides trigonometric functions such as `sin`, `cos`, and `tan`.
11. `np.deg2rad()` and `np.rad2deg()` convert between degrees and radians.
12. `np.min()` and `np.max()` identify extreme values.
13. Basic statistical calculations can also be performed using NumPy.
14. Mathematical models used in electronics and semiconductor science can be implemented directly using NumPy functions.
15. Vectorized calculations allow the same mathematical model to be applied efficiently to many measurements.

---

9.42 Key Takeaway

> **NumPy allows mathematical notation to translate naturally into scientific Python. Once experimental measurements are represented as arrays, equations, transformations, and models can be applied to many observations with concise and readable code.**

The next chapter focuses on **statistical operations with NumPy**, including mean, variance, standard deviation, minimum, maximum, and basic interpretation of experimental measurements.
