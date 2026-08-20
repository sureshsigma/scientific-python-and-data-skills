# Chapter 11 — Linear Spacing and Numerical Calculations

## 11.1 Learning Objectives

After completing this chapter, you should be able to:

- generate numerical sequences using NumPy;
- distinguish between `np.arange()` and `np.linspace()`;
- generate evenly spaced values for scientific calculations;
- understand why controlled numerical sampling is important;
- perform vectorized calculations using NumPy arrays;
- apply mathematical equations to complete arrays;
- generate scientific input values for models and experiments;
- prepare numerical data that can later be visualized;
- connect numerical calculations with simple circuit and semiconductor applications.

---

11.2 From Data to Numerical Experiments

In scientific computing, we often need more than the measurements that have already been collected.

Sometimes we need to generate numerical values before performing an analysis.

For example, suppose we want to study the relationship:

$$
I=\frac{V}{R}.
$$

If:

$$
R=100\,\Omega,
$$

we may want to calculate current for:

$$
V=0,0.5,1.0,1.5,\ldots,5.0\ \mathrm{V}.
$$

Instead of entering every value manually, NumPy can generate the values.

The workflow becomes:

```text
Define scientific problem
        ↓
Generate numerical inputs
        ↓
Apply mathematical model
        ↓
Obtain numerical outputs
        ↓
Visualize / analyze
```

This is a fundamental pattern in scientific Python.

---

11.3 Generating Numerical Sequences

Python provides several ways to generate sequences.

For scientific computing, NumPy provides:

```python
np.arange()
```

and:

```python
np.linspace()
```

Both generate numerical arrays, but they answer different questions.

The key distinction is:

> `np.arange()` is primarily step-based.

> `np.linspace()` is primarily point-count-based.

Understanding this difference is important when creating scientific datasets.

---

11.4 `np.arange()`

The basic form is:

```python
np.arange(start, stop, step)
```

For example:

```python
import numpy as np

x = np.arange(0, 10, 2)

print(x)
```

Output:

```text
[0 2 4 6 8]
```

The sequence begins at:

$$
0
$$

and advances by:

$$
2.
$$

The stop value is generally excluded.

Therefore, `10` is not included.

---

11.5 Understanding `start`, `stop`, and `step`

Consider:

```python
x = np.arange(1, 10, 2)
```

The parameters mean:

```text
start = 1
stop  = 10
step  = 2
```

The result is:

```text
[1 3 5 7 9]
```

The sequence follows:

$$
x_k=x_0+k\Delta x,
$$

where:

$$
x_0=1
$$

and:

$$
\Delta x=2.
$$

The sequence stops before reaching the specified stop value.

---

11.6 Generating Voltage Values with `arange()`

Suppose we want voltage values from:

$$
0\,\mathrm{V}
$$

to less than:

$$
5\,\mathrm{V}
$$

in steps of:

$$
0.5\,\mathrm{V}.
$$

We can write:

```python
V = np.arange(0, 5, 0.5)

print(V)
```

The resulting array contains values such as:

```text
[0.  0.5 1.  1.5 2.  2.5 3.  3.5 4.  4.5]
```

Notice that 5 V is not included.

If the endpoint must be included, `linspace()` is often more convenient.

---

11.7 Floating-Point Considerations with `arange()`

Consider:

```python
x = np.arange(0, 1, 0.1)

print(x)
```

The result may appear as:

```text
[0.  0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9]
```

However, floating-point numbers are represented approximately inside a computer.

Therefore, calculations involving floating-point step sizes should not rely on exact decimal representation.

For example:

```python
x = np.arange(0, 1, 0.1)

print(x[-1])
```

may produce a value that is very close to, but not mathematically identical to, \(0.9\).

This is one reason `np.linspace()` is often preferred when a fixed number of points or exact endpoints are important.

---

11.8 `np.linspace()`

The basic form is:

```python
np.linspace(start, stop, number_of_points)
```

For example:

```python
x = np.linspace(0, 10, 6)

print(x)
```

Output:

```text
[ 0.  2.  4.  6.  8. 10.]
```

Here:

- start = 0;
- stop = 10;
- number of points = 6.

The stop value is included by default.

---

11.9 Understanding `linspace()`

Suppose:

```python
x = np.linspace(0, 1, 11)
```

The array contains 11 equally spaced values:

```text
0.0
0.1
0.2
...
0.9
1.0
```

The spacing is:

$$
\Delta x
=
\frac{x_{\max}-x_{\min}}{N-1}.
$$

For this example:

$$
\Delta x
=
\frac{1-0}{11-1}
=
0.1.
$$

The important point is that `linspace()` guarantees the requested number of points between the endpoints.

---

11.10 Generating Voltage Values with `linspace()`

Suppose we want exactly 11 voltage values between:

$$
0\,\mathrm{V}
$$

and:

$$
5\,\mathrm{V}.
$$

Use:

```python
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

This is useful when a scientific model needs to be evaluated at a controlled number of points.

---

11.11 `arange()` versus `linspace()`

The distinction can be summarized as:

| Requirement | Recommended function |
| --- | --- |
| Specify step size | `np.arange()` |
| Specify number of points | `np.linspace()` |
| Include endpoint reliably | `np.linspace()` |
| Generate integer-like sequences | `np.arange()` |
| Generate controlled sampling points | `np.linspace()` |
| Scientific model evaluation | Often `np.linspace()` |

A useful rule is:

> If you know the step size, think `arange()`.

> If you know the number of points, think `linspace()`.

---

11.12 Why Evenly Spaced Values Matter

Suppose a mathematical model is evaluated only at:

```text
0, 1, 5, 10
```

The spacing is uneven.

If the objective is to study the behavior of a function across an interval, uneven sampling may hide important behavior.

Instead, we can use:

```python
x = np.linspace(0, 10, 101)
```

This produces 101 equally spaced values.

The spacing is:

$$
\Delta x
=
\frac{10-0}{100}
=
0.1.
$$

A finer sampling produces a more detailed numerical representation of the function.

---

11.13 Number of Points and Resolution

Suppose:

```python
x1 = np.linspace(0, 10, 11)
```

and:

```python
x2 = np.linspace(0, 10, 101)
```

Both cover the same interval.

But:

```text
x1 → 11 points
x2 → 101 points
```

The second array provides a finer numerical sampling.

This does not automatically make the underlying scientific model more accurate.

It simply evaluates the model at more locations.

This distinction is important:

> More numerical points do not necessarily mean more physical accuracy.

---

11.14 Generating Time Values

Suppose an experiment lasts:

$$
10\,\mathrm{s}.
$$

We want measurements at 101 equally spaced time points.

```python
t = np.linspace(0, 10, 101)
```

The spacing is:

$$
\Delta t
=
\frac{10}{100}
=
0.1\,\mathrm{s}.
$$

The resulting array can be used as the input to a time-dependent model.

For example:

$$
y(t)=e^{-t}.
$$

---

11.15 Generating a Scientific Model

Consider:

$$
y=e^{-t}.
$$

We can write:

```python
t = np.linspace(0, 10, 101)

y = np.exp(-t)
```

The two arrays represent:

```text
t → input values
y → calculated model values
```

This creates a numerical representation of the function.

The next stage could be visualization.

---

11.16 Vectorized Calculations

A major advantage of NumPy is that mathematical operations can be applied to entire arrays.

Consider:

```python
x = np.array([1, 2, 3, 4, 5])
```

We can calculate:

```python
y = 2 * x + 1
```

Output:

```text
[ 3  5  7  9 11]
```

Mathematically:

$$
y_i=2x_i+1.
$$

The calculation is performed for every element.

We do not need to write:

```python
for value in x:
    ...
```

for this operation.

---

11.17 Why Is This Called Vectorized Computation?

The array:

$$
\mathbf{x}
=
\begin{bmatrix}
1\\
2\\
3\\
4\\
5
\end{bmatrix}
$$

is treated as a numerical object.

The operation:

$$
2\mathbf{x}+1
$$

produces:

$$
\begin{bmatrix}
3\\
5\\
7\\
9\\
11
\end{bmatrix}.
$$

Python code:

```python
y = 2 * x + 1
```

closely resembles the mathematical expression.

This makes scientific programs both concise and readable.

---

11.18 Vectorized Ohm's Law

Ohm's law is:

$$
I=\frac{V}{R}.
$$

Suppose:

$$
R=100\,\Omega.
$$

Generate voltage values:

```python
V = np.linspace(0, 5, 11)
```

Calculate current:

```python
R = 100

I = V / R
```

The complete calculation is:

```python
import numpy as np

V = np.linspace(0, 5, 11)

R = 100

I = V / R

print(V)
print(I)
```

The model is evaluated for every voltage value.

---

11.19 Vectorized Power Calculation

Electrical power is:

$$
P=VI.
$$

Suppose:

```python
V = np.linspace(0, 5, 11)
R = 100

I = V / R
P = V * I
```

Since:

$$
I=\frac{V}{R},
$$

we can also write:

$$
P=\frac{V^2}{R}.
$$

Python:

```python
P = V ** 2 / R
```

Both approaches produce the same result, apart from normal floating-point representation.

---

11.20 Comparing Two Equivalent Equations

Consider:

$$
P=VI
$$

and:

$$
P=\frac{V^2}{R}.
$$

Python:

```python
P1 = V * I
P2 = V ** 2 / R

print(np.allclose(P1, P2))
```

Output:

```text
True
```

`np.allclose()` checks whether corresponding values are approximately equal within numerical tolerance.

This is useful when validating numerical implementations.

---

11.21 Vectorized Unit Conversion

Suppose voltage is recorded in millivolts:

```python
V_mV = np.array([100, 200, 300, 400, 500])
```

Convert to volts:

$$
V=V_{\mathrm{mV}}\times10^{-3}.
$$

Python:

```python
V = V_mV * 1e-3
```

This converts the complete array at once.

Similarly, micrometres can be converted to metres:

```python
length_m = length_um * 1e-6
```

Vectorized unit conversion is common in scientific data preparation.

---

11.22 Temperature Conversion

Suppose temperature is given in Celsius:

```python
T_C = np.array([20, 25, 30, 35, 40])
```

Convert to Kelvin:

$$
T_K=T_C+273.15.
$$

Python:

```python
T_K = T_C + 273.15
```

Output:

```text
[293.15 298.15 303.15 308.15 313.15]
```

The operation is applied to every observation.

---

11.23 Generating Temperature Inputs

Suppose a model must be evaluated from:

$$
300\,\mathrm{K}
$$

to:

$$
400\,\mathrm{K}.
$$

Generate 21 equally spaced values:

```python
T = np.linspace(300, 400, 21)
```

The temperature interval is:

$$
100\,\mathrm{K}
$$

and the spacing is:

$$
\Delta T
=
\frac{400-300}{20}
=
5\,\mathrm{K}.
$$

The array can then be used as input to a temperature-dependent model.

---

11.24 A Temperature-Dependent Model

Suppose a simple model is:

$$
R(T)
=
R_0
\left[
1+\alpha(T-T_0)
\right].
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

Generate temperatures:

```python
T = np.linspace(300, 400, 21)
```

Calculate:

```python
R0 = 100
alpha = 0.004
T0 = 300

R = R0 * (1 + alpha * (T - T0))
```

The model is evaluated at all 21 temperatures.

---

11.25 Generating Input Values for Semiconductor Models

Many semiconductor relationships contain exponential functions.

Consider the simplified model:

$$
I
=
I_S
\left(
e^{\frac{V}{nV_T}}-1
\right).
$$

We can generate voltage values:

```python
V = np.linspace(0, 0.7, 200)
```

and then evaluate:

```python
I = Is * (np.exp(V / (n * Vt)) - 1)
```

The important structure is:

```text
Generate V
    ↓
Evaluate equation
    ↓
Obtain I
```

The same pattern can be applied to many scientific models.

---

11.26 Numerical Sampling of a Function

Consider:

$$
f(x)=x^2.
$$

If we generate:

```python
x = np.linspace(-5, 5, 11)
```

and calculate:

```python
y = x ** 2
```

we obtain numerical samples of the function.

The computer does not store the complete continuous curve.

Instead, it stores a finite set of numerical points:

$$
(x_1,f(x_1)),
(x_2,f(x_2)),
\ldots,
(x_n,f(x_n)).
$$

Visualization software can then use these points to represent the function graphically.

---

11.27 Sampling Density

Consider two arrays:

```python
x_coarse = np.linspace(0, 10, 11)
```

and:

```python
x_fine = np.linspace(0, 10, 1001)
```

The second array has much finer sampling.

For a smooth function, the resulting graph may appear smoother when more points are used.

However, there is a practical trade-off.

More points mean:

- more numerical calculations;
- more memory;
- potentially longer processing time.

For most introductory scientific calculations, a reasonable number of points is sufficient.

---

11.28 Numerical Calculation versus Measurement

It is important to distinguish between:

**Measured data**

and:

**Generated numerical data.**

For example:

```python
V_measured = np.array([0.10, 0.21, 0.31, 0.39])
```

represents observations from an experiment.

Whereas:

```python
V_model = np.linspace(0, 0.5, 100)
```

represents numerical input values generated by the program.

The second dataset is not experimental evidence.

It is a computational grid used to evaluate or visualize a model.

This distinction should always be clear in scientific work.

---

11.29 Combining Generated and Measured Data

A common scientific workflow uses both.

Suppose measured data is:

```python
V_measured = np.array([0.1, 0.2, 0.3, 0.4, 0.5])
```

We can generate a smooth model grid:

```python
V_model = np.linspace(0.1, 0.5, 100)
```

Then calculate the model:

```python
I_model = V_model / 100
```

The two datasets have different purposes:

```text
Measured values
    ↓
Experimental evidence

Generated values
    ↓
Model evaluation
```

This distinction becomes especially important when experimental data is compared with a theoretical model.

---

11.30 Numerical Precision

Computers use finite-precision numerical representations.

For example:

```python
x = 0.1 + 0.2

print(x)
```

may produce:

```text
0.30000000000000004
```

Mathematically:

$$
0.1+0.2=0.3.
$$

The small difference is due to the binary representation of floating-point numbers.

This is not a failure of arithmetic.

It is a consequence of finite numerical representation.

---

11.31 Comparing Floating-Point Results

Avoid relying on:

```python
a == b
```

when comparing floating-point calculations that are expected to be mathematically equal.

Instead, NumPy provides:

```python
np.isclose()
```

For example:

```python
a = 0.1 + 0.2
b = 0.3

print(np.isclose(a, b))
```

Output:

```text
True
```

For arrays:

```python
np.allclose(array1, array2)
```

can be used.

These functions are useful when validating numerical calculations.

---

11.32 Numerical Calculations with Multiple Arrays

Suppose:

```python
V = np.array([1, 2, 3, 4, 5])
R = np.array([100, 100, 200, 200, 250])
```

Calculate:

```python
I = V / R
```

Mathematically:

$$
I_i=\frac{V_i}{R_i}.
$$

The corresponding values are calculated element by element.

This is useful when each observation has its own associated parameter.

---

11.33 Broadcasting with a Scalar

Suppose:

```python
V = np.array([1, 2, 3, 4, 5])
R = 100
```

Then:

```python
I = V / R
```

works even though `V` is an array and `R` is a scalar.

NumPy effectively applies the scalar to each element:

$$
\begin{bmatrix}
1\\
2\\
3\\
4\\
5
\end{bmatrix}
\div100.
$$

This behavior is called **broadcasting**.

A detailed treatment of broadcasting will be introduced when more complex array structures are required.

---

11.34 Generating Scientific Input Grids

A useful scientific pattern is:

```python
x = np.linspace(x_min, x_max, n)
```

where:

- `x_min` = lower limit;
- `x_max` = upper limit;
- `n` = number of points.

For example:

```python
V = np.linspace(0, 5, 501)
```

creates 501 voltage values between 0 V and 5 V.

The spacing is:

$$
\Delta V
=
\frac{5-0}{500}
=
0.01\,\mathrm{V}.
$$

This provides a fine numerical grid for evaluating a model.

---

11.35 A General Scientific Pattern

Many numerical problems can be expressed as:

```python
import numpy as np

x = np.linspace(x_min, x_max, n)

y = model(x)
```

For example:

```python
x = np.linspace(0, 10, 101)

y = np.exp(-x)
```

or:

```python
V = np.linspace(0, 5, 101)

I = V / 100
```

or:

```python
T = np.linspace(300, 400, 101)

R = R0 * (1 + alpha * (T - T0))
```

This pattern is central to scientific programming.

---

11.36 Preparing Data for Visualization

The arrays generated in this chapter are already suitable for plotting.

For example:

```python
V = np.linspace(0, 5, 101)
I = V / 100
```

The arrays can later be passed to Matplotlib:

```python
plt.plot(V, I)
```

The complete workflow is:

```text
Generate input values
        ↓
Perform numerical calculation
        ↓
Obtain output array
        ↓
Check values
        ↓
Visualize
```

This is why numerical array generation should be understood before beginning scientific plotting.

---

11.37 Worked Example: Ohm's Law Dataset

Generate a complete dataset for:

$$
R=100\,\Omega
$$

over:

$$
0\leq V\leq5\,\mathrm{V}.
$$

Use 11 points.

```python
import numpy as np

V = np.linspace(0, 5, 11)

R = 100

I = V / R

P = V * I

print("Voltage (V):")
print(V)

print("Current (A):")
print(I)

print("Power (W):")
print(P)
```

The program has generated three related numerical quantities:

$$
V,
\qquad
I,
\qquad
P.
$$

The relationship is:

$$
I=\frac{V}{R},
$$

and:

$$
P=VI.
$$

This dataset is ready for visualization in a later chapter.

---

11.38 Worked Example: Temperature Dataset

Generate temperature values from:

$$
300\,\mathrm{K}
$$

to:

$$
350\,\mathrm{K}
$$

using 11 points.

```python
import numpy as np

T = np.linspace(300, 350, 11)

R0 = 100
alpha = 0.004
T0 = 300

R = R0 * (1 + alpha * (T - T0))

print("Temperature (K):")
print(T)

print("Resistance (ohm):")
print(R)
```

The generated dataset can later be plotted as:

$$
R \text{ versus } T.
$$

---

11.39 Worked Example: Exponential Semiconductor Model

Consider:

$$
I
=
I_S
\left(
e^{\frac{V}{nV_T}}-1
\right).
$$

Use:

$$
I_S=10^{-12}\,\mathrm{A},
$$

$$
n=1,
$$

and:

$$
V_T=0.02585\,\mathrm{V}.
$$

Generate 101 voltage values:

```python
import numpy as np

Is = 1e-12
n = 1
Vt = 0.02585

V = np.linspace(0, 0.7, 101)

I = Is * (np.exp(V / (n * Vt)) - 1)

print(V[:5])
print(I[:5])
```

Only the first few values are displayed.

The complete arrays contain 101 numerical observations.

These arrays can later be visualized to examine the shape of the model.

---

11.40 Common Beginner Mistakes

### Mistake 1 — Assuming `arange()` includes the stop value

Usually:

```python
np.arange(0, 5, 1)
```

produces:

```text
[0 1 2 3 4]
```

not 5.

### Mistake 2 — Confusing step size with number of points

In:

```python
np.arange(0, 5, 0.5)
```

`0.5` is the step.

In:

```python
np.linspace(0, 5, 11)
```

`11` is the number of points.

### Mistake 3 — Using too few points for a smooth model

A complex curve may appear poorly represented if it is evaluated at only a few points.

### Mistake 4 — Assuming more points mean more physical accuracy

Increasing the numerical grid does not improve the quality of the underlying physical model.

### Mistake 5 — Treating generated values as experimental observations

Values produced by `linspace()` are computational inputs, not measurements.

### Mistake 6 — Comparing floating-point numbers with exact equality

Use:

```python
np.isclose()
```

or:

```python
np.allclose()
```

when appropriate.

### Mistake 7 — Ignoring units

A numerical value such as:

```python
300
```

does not automatically mean 300 K.

The scientific context and units must be explicitly defined.

---

11.41 Exercises

### Exercise 1 — `arange()`

Use `np.arange()` to generate:

```text
0, 2, 4, 6, 8
```

---

### Exercise 2 — `linspace()`

Generate 11 equally spaced values between:

$$
0
$$

and:

$$
10.
$$

Verify the spacing.

---

### Exercise 3 — Voltage Grid

Generate 101 equally spaced voltage values between:

$$
0
$$

and:

$$
5\,\mathrm{V}.
$$

Calculate the voltage spacing.

---

### Exercise 4 — Ohm's Law

For:

$$
R=100\,\Omega,
$$

generate current values using:

$$
I=\frac{V}{R}.
$$

Use the voltage grid from Exercise 3.

---

### Exercise 5 — Power

Using the same arrays, calculate:

$$
P=VI.
$$

Also calculate:

$$
P=\frac{V^2}{R}.
$$

Use `np.allclose()` to verify that the results agree.

---

### Exercise 6 — Temperature

Generate 21 equally spaced temperatures between:

$$
300\,\mathrm{K}
$$

and:

$$
400\,\mathrm{K}.
$$

Calculate the spacing.

---

### Exercise 7 — Temperature-Dependent Resistance

Use:

$$
R(T)
=
R_0[1+\alpha(T-T_0)]
$$

with:

$$
R_0=100\,\Omega,
\quad
\alpha=0.004\,\mathrm{K}^{-1},
\quad
T_0=300\,\mathrm{K}.
$$

Calculate resistance for all temperatures from Exercise 6.

---

### Exercise 8 — Exponential Model

Generate 101 values between:

$$
0
$$

and:

$$
5.
$$

Calculate:

$$
y=e^x.
$$

---

### Exercise 9 — Compare Sampling

Generate:

```python
x1 = np.linspace(0, 10, 11)
```

and:

```python
x2 = np.linspace(0, 10, 101)
```

Calculate:

$$
y=x^2
$$

for both arrays.

Compare the number of values.

---

### Exercise 10 — Scientific Input versus Measurement

Create:

```python
V_measured = np.array([0.1, 0.2, 0.31, 0.39, 0.5])
```

and:

```python
V_model = np.linspace(0.1, 0.5, 100)
```

Explain why these two arrays should not be described as the same type of data.

---

11.42 Think and Apply

Suppose you want to study a mathematical model over:

$$
0\leq x\leq100.
$$

You know that you need exactly 501 evaluation points.

Which function would you choose?

```python
np.arange()
```

or:

```python
np.linspace()
```

Why?

Now consider a different situation:

> You need values starting at 0, increasing by exactly 2, and stopping before 100.

Which function is more natural?

---

11.43 Chapter Summary

In this chapter, we learned that:

1. Scientific computing often requires generating numerical input values.
2. `np.arange()` generates sequences primarily by specifying a step size.
3. `np.linspace()` generates sequences by specifying the number of points.
4. `np.linspace()` includes the endpoint by default.
5. Evenly spaced values are useful for evaluating mathematical and scientific models.
6. Vectorized calculations apply mathematical operations to complete arrays.
7. NumPy allows scientific equations to be implemented directly using arrays.
8. Unit conversions can be performed efficiently using vectorized operations.
9. Generated numerical grids are computational inputs, not experimental measurements.
10. `np.isclose()` and `np.allclose()` are useful when comparing floating-point results.
11. Numerical sampling density affects the representation of a model but does not automatically improve physical accuracy.
12. Generated voltage, current, temperature, and other arrays can be prepared for later visualization.
13. The combination of numerical input generation and vectorized calculation forms a basic scientific computing workflow.

---

11.44 Key Takeaway

> **Scientific Python often begins by creating the numerical space in which a problem will be studied. `np.linspace()` and `np.arange()` provide controlled ways to generate that space, while NumPy's vectorized operations allow scientific equations to be evaluated efficiently across the resulting arrays.**

The next chapter connects these numerical techniques directly to **scientific and semiconductor device data**, including voltage, current, temperature, resistance, repeated measurements, and preparation of datasets for visualization.
