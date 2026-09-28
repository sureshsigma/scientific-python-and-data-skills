# Chapter 23 — Solving Scientific Equations

## 23.1 Introduction

In scientific experiments, we often know the relationship between physical quantities but need to find the value of an unknown quantity.

For example:

- At what voltage does a diode current reach a specified value?
- At what temperature does a material property reach a particular value?
- What value of a variable makes a measured model equal to zero?
- At what point do two scientific curves intersect?

These are examples of **scientific equations**.

Python and SciPy can solve many such equations numerically.

The main idea is:

$$
\boxed{\text{Scientific Equation}\rightarrow\text{Define Function}\rightarrow\text{Solve Numerically}\rightarrow\text{Check Result}}
$$

---

## 23.2 What Is an Equation?

An equation states that two expressions are equal.

For example,

$$
2x+5=15
$$

We want to find the value of \(x\) that makes the equation true.

Rearranging manually,

$$
2x=10
$$

so,

$$
x=5
$$

For simple equations, we can solve them analytically.

However, scientific equations are often more complicated.

For example,

$$
e^x-5x=0
$$

or

$$
x^3-4x-1=0
$$

These may be difficult to solve directly.

This is where numerical methods are useful.

---

## 23.3 Convert the Equation into a Function

SciPy numerical solvers generally work with an equation written in the form

$$
f(x)=0
$$

For example,

$$
2x+5=15
$$

can be written as

$$
2x+5-15=0
$$

Therefore,

$$
f(x)=2x-10
$$

The solution is the value of \(x\) for which

$$
f(x)=0
$$

### Python

```python
def f(x):
    return 2*x - 10
```

The important idea is:

> Move everything to one side so that the equation becomes \(f(x)=0\).

---

## 23.4 Solving a Simple Equation with SciPy

SciPy provides numerical solvers through `scipy.optimize`.

For a single-variable equation, one useful function is `root_scalar()`.

```python
from scipy.optimize import root_scalar

def f(x):
    return 2*x - 10

result = root_scalar(f, bracket=[0, 10])

print(result.root)
```

Output:

```text
5.0
```

The solution is therefore

$$
\boxed{x=5}
$$

---

## 23.5 What Is a Bracket?

The `bracket` tells the numerical solver where to search for the solution.

```python
bracket=[0, 10]
```

means:

> Search for a root between 0 and 10.

A **root** is a value of \(x\) for which

$$
f(x)=0
$$

For the example,

$$
f(0)=-10
$$

and

$$
f(10)=10
$$

The function changes sign between the two points, so a root exists inside the interval.

---

## 23.6 A Scientific Example — Finding a Voltage

Suppose an experimental device follows the approximate relationship

$$
I=2V+0.1
$$

where:

- \(V\) = voltage in volts
- \(I\) = current in mA

Suppose we want to find the voltage at which

$$
I=5\text{ mA}
$$

Substitute the required current:

$$
2V+0.1=5
$$

Write the equation as

$$
2V+0.1-5=0
$$

Therefore,

$$
f(V)=2V-4.9
$$

### Python

```python
from scipy.optimize import root_scalar

def f(V):
    return 2*V + 0.1 - 5

result = root_scalar(f, bracket=[0, 5])

print("Voltage =", result.root, "V")
```

Output:

```text
Voltage = 2.45 V
```

Therefore,

$$
\boxed{V\approx2.45\text{ V}}
$$

### Scientific interpretation

The calculated voltage is the voltage at which the model predicts a current of approximately 5 mA.

---

## 23.7 Why Numerical Solving Is Useful

In real semiconductor work, the equation may not be as simple as

$$
I=2V+0.1
$$

For example, diode current can be represented approximately by

$$
I=I_s\left(e^{V/(nV_T)}-1\right)
$$

where \(I_s\), \(n\), and \(V_T\) are device-related quantities.

If we want to find the voltage corresponding to a particular current, rearranging the equation may not always be convenient.

A numerical solver can search for the voltage directly.

---

## 23.8 Diode Example

Suppose

$$
I_s=10^{-9}\text{ A}
$$

$$
n=2
$$

$$
V_T=0.026\text{ V}
$$

and we want to find the voltage corresponding to

$$
I=1\text{ mA}
$$

Define

$$
f(V)=I_s\left(e^{V/(nV_T)}-1\right)-I
$$

### Python

```python
import numpy as np
from scipy.optimize import root_scalar

Is = 1e-9
n = 2
Vt = 0.026
I_target = 1e-3

def f(V):
    return Is * (np.exp(V / (n * Vt)) - 1) - I_target

result = root_scalar(f, bracket=[0, 1])

print("Voltage =", result.root, "V")
```

The numerical solution is approximately

```text
Voltage = 0.717 V
```

Thus,

$$
\boxed{V\approx0.717\text{ V}}
$$

### Interpretation

The numerical solution gives the voltage predicted by the diode model for a current of 1 mA.

This is a useful example of how a physical equation can be converted into a numerical problem.

---

## 23.9 Checking the Solution

A numerical answer should always be checked.

Suppose Python gives

$$
V=0.717\text{ V}
$$

Substitute this value back into the original equation.

```python
V = result.root

I_check = Is * (np.exp(V / (n * Vt)) - 1)

print("Calculated current =", I_check, "A")
```

The result should be close to

```text
0.001 A
```

or

$$
1\text{ mA}
$$

This gives us confidence that the numerical solution is correct.

### Important habit

$$
\boxed{\text{Solve}\rightarrow\text{Substitute Back}\rightarrow\text{Check}}
$$

---

## 23.10 What If There Are Multiple Solutions?

Some equations can have more than one solution.

For example,

$$
x^2-4=0
$$

has two roots:

$$
x=-2
$$

and

$$
x=2
$$

A numerical solver using one bracket may find one root depending on the interval supplied.

### Python

```python
from scipy.optimize import root_scalar

def f(x):
    return x**2 - 4

result1 = root_scalar(f, bracket=[-3, 0])
result2 = root_scalar(f, bracket=[0, 3])

print(result1.root)
print(result2.root)
```

Output:

```text
-2.0
2.0
```

Therefore, the search interval matters.

---

## 23.11 Numerical Solving Is a Search Process

It is useful to think of numerical solving as a search.

Suppose

$$
f(x)=0
$$

We want to find the value of \(x\) where the function crosses zero.

Conceptually:

$$
\boxed{
\text{Choose interval}
\rightarrow
\text{Evaluate function}
\rightarrow
\text{Search for zero}
\rightarrow
\text{Obtain root}
\rightarrow
\text{Check}
}
$$

The computer performs the numerical calculations.

---

## 23.12 Root Finding vs Interpolation

Root finding and interpolation are related but different.

### Interpolation

We know experimental values and estimate a value between them.

Example:

$$
V=0.2,\ 0.3,\ 0.4
$$

and corresponding current values are known.

We estimate the current at

$$
V=0.25
$$

### Root finding

We have an equation or model and want to find where it satisfies a condition.

For example,

$$
I(V)-5=0
$$

to find the voltage where

$$
I=5\text{ mA}
$$

Therefore:

| Task | Main question |
|---|---|
| Interpolation | What is the value between known data points? |
| Root finding | Where does the function satisfy a condition? |

---

## 23.13 Common Mistakes

### Mistake 1 — Not converting to \(f(x)=0\)

Incorrect thinking:

```python
def f(x):
    return 2*x + 5
```

when the actual equation is

$$
2x+5=15
$$

Correct:

```python
def f(x):
    return 2*x + 5 - 15
```

---

### Mistake 2 — Choosing an unsuitable bracket

For example:

```python
root_scalar(f, bracket=[20, 30])
```

may fail if there is no root in that interval.

Always think about the physical or mathematical range of the variable.

---

### Mistake 3 — Not checking the result

Do not stop after obtaining a number.

Substitute the solution back into the original equation and verify it.

---

### Mistake 4 — Ignoring units

A numerical value without units may be scientifically meaningless.

For example:

$$
V=0.717
$$

should be reported as

$$
V=0.717\text{ V}
$$

when the variable represents voltage.

---

## 23.14 Complete Semiconductor Example

Suppose the following diode model is given:

$$
I=I_s\left(e^{V/(nV_T)}-1\right)
$$

We want the voltage corresponding to

$$
I=2\text{ mA}
$$

### Step 1 — Define constants

```python
import numpy as np
from scipy.optimize import root_scalar

Is = 1e-9
n = 2
Vt = 0.026
I_target = 2e-3
```

### Step 2 — Define the equation

```python
def f(V):
    return Is * (np.exp(V / (n * Vt)) - 1) - I_target
```

### Step 3 — Solve

```python
result = root_scalar(f, bracket=[0, 1])

V_solution = result.root
```

### Step 4 — Print the result

```python
print("Voltage =", V_solution, "V")
```

### Step 5 — Check

```python
I_check = Is * (np.exp(V_solution / (n * Vt)) - 1)

print("Current from model =", I_check, "A")
```

The calculated current should be close to

$$
2\times10^{-3}\text{ A}
$$

### Scientific workflow

$$
\boxed{
\text{Physical Model}
\rightarrow
\text{Equation}
\rightarrow
f(x)=0
\rightarrow
\text{Numerical Solver}
\rightarrow
\text{Check}
\rightarrow
\text{Interpret}
}
$$

---

## 23.15 Key Points

- Many scientific problems can be written as \(f(x)=0\).
- SciPy provides numerical methods for solving equations.
- `scipy.optimize.root_scalar()` can be used for one-dimensional root finding.
- A bracket defines the interval in which the solver searches.
- The solution should always be checked by substitution.
- Some equations can have multiple roots.
- Units and physical meaning must be considered.
- Root finding is different from interpolation.
- Numerical solving is especially useful when equations are difficult to solve analytically.

---

## 23.16 Quick Practice

### Question 1

Write the following equation in the form \(f(x)=0\):

$$
3x+4=16
$$

### Question 2

What does the following mean?

```python
bracket=[0, 5]
```

### Question 3

Why should a numerical solution be checked by substituting it back into the original equation?

### Question 4

Find the root of

$$
x^2-9=0
$$

using suitable intervals.

### Question 5

A device model gives

$$
I=4V+0.2
$$

Find the voltage corresponding to

$$
I=2\text{ A}
$$

using numerical root finding.

---

## 23.17 Hands-on Activity

Create a Python program that models a simple device relationship:

$$
I=3V+0.1
$$

Use `scipy.optimize.root_scalar()` to find the voltage corresponding to:

$$
I=1\text{ A}
$$

Then:

1. Print the calculated voltage.
2. Substitute the voltage back into the model.
3. Print the calculated current.
4. Compare it with the target current.
5. Report the result with units.

### Extension

Change the target current to:

$$
I=0.5,\ 1.0,\ 1.5,\ 2.0\text{ A}
$$

and solve for the corresponding voltage each time.

---

## 23.18 Chapter Summary

Scientific experiments often involve equations that cannot be solved conveniently by hand.

Python with SciPy allows us to solve such equations numerically.

The essential workflow is:

$$
\boxed{
\text{Define the scientific equation}
\rightarrow
\text{Convert to }f(x)=0
\rightarrow
\text{Choose a search interval}
\rightarrow
\text{Solve}
\rightarrow
\text{Check}
\rightarrow
\text{Interpret}
}
$$

The important idea is not simply obtaining a numerical answer.

The goal is to obtain a **scientifically meaningful solution and understand what that solution represents**.
