# Chapter 2 — Python as a Scientific Calculator

## 2.1 Learning Objectives

After completing this chapter, you should be able to:

- use Python to perform arithmetic calculations;
- distinguish between integers and floating-point numbers;
- create variables and assign values;
- use Python arithmetic operators;
- apply the order of operations;
- represent scientific notation in Python;
- translate mathematical expressions into Python expressions;
- use Python to solve basic electronics and scientific calculations;
- keep track of physical units when performing calculations;
- recognize common mistakes in numerical calculations.

---

2.2 Python as a Scientific Calculator

Python can be used as a calculator for ordinary arithmetic as well as scientific calculations.

For example:

    12 + 8

can be evaluated directly in Python.

```python
12 + 8
```

Output:

```text
20
```

Python can also evaluate expressions involving multiplication and division.

```python
15 * 4
```

Output:

```text
60
```

```python
100 / 4
```

Output:

```text
25.0
```

The important idea is that Python evaluates an expression and returns a result.

A mathematical expression such as

$$
\frac{20+10}{5}
$$

can be written in Python as:

```python
(20 + 10) / 5
```

Output:

```text
6.0
```

Python therefore provides a direct connection between mathematical expressions and computational expressions.

---

2.3 Numbers in Python

Scientific calculations mainly involve numerical values.

Python provides several numerical data types. At this stage, two are particularly important:

- integers;
- floating-point numbers.

2.3.1 Integers

An integer is a whole number without a decimal part.

Examples include:

```python
5
100
-25
0
```

For example:

```python
resistance = 100
print(resistance)
```

Output:

```text
100
```

2.3.2 Floating-Point Numbers

A floating-point number contains a decimal component.

Examples include:

```python
5.0
0.25
3.14159
-2.5
```

For example:

```python
voltage = 5.0
print(voltage)
```

Output:

```text
5.0
```

Both integers and floating-point numbers can be used in numerical calculations.

---

2.4 Variables and Assignment

A variable is a name used to refer to a value.

Consider:

```python
V = 5
```

Here:

- `V` is the variable name;
- `5` is the value assigned to the variable.

The symbol `=` in Python is called the assignment operator.

It means:

> Assign the value on the right to the variable on the left.

For example:

```python
R = 100
I = 0.05
```

We can then use these variables in calculations.

```python
V = 5
R = 100

I = V / R

print(I)
```

Output:

```text
0.05
```

The same variable can be assigned a new value.

```python
V = 5
print(V)

V = 10
print(V)
```

Output:

```text
5
10
```

The variable `V` now refers to the value `10`.

---

2.5 Choosing Meaningful Variable Names

Variable names should communicate what the value represents.

Prefer:

```python
voltage = 5
resistance = 100
current = 0.05
```

over:

```python
a = 5
b = 100
c = 0.05
```

The second version may work, but the first version is easier to understand.

Scientific programs should be written so that another person can read and understand them.

Good variable names are particularly important when a program contains many measurements or calculations.

For example:

```python
applied_voltage = 5
resistance = 220
current = applied_voltage / resistance
```

is more informative than:

```python
x = 5
y = 220
z = x / y
```

---

2.6 Arithmetic Operators

Python provides operators for basic arithmetic.

| Mathematical operation | Python operator | Example |
| --- | --- | --- |
| Addition | `+` | `a + b` |
| Subtraction | `-` | `a - b` |
| Multiplication | `*` | `a * b` |
| Division | `/` | `a / b` |
| Exponentiation | `**` | `a ** b` |
| Remainder | `%` | `a % b` |

2.6.1 Addition

Mathematically,

$$
a+b
$$

is written in Python as:

```python
a + b
```

Example:

```python
8 + 5
```

Output:

```text
13
```

2.6.2 Subtraction

```python
10 - 4
```

Output:

```text
6
```

2.6.3 Multiplication

Python uses `*` for multiplication.

```python
6 * 7
```

Output:

```text
42
```

The symbol `×` is not used for multiplication in Python.

2.6.4 Division

Python uses `/` for division.

```python
10 / 4
```

Output:

```text
2.5
```

Notice that Python returns a floating-point result.

2.6.5 Exponentiation

The mathematical expression

$$
2^3
$$

is written in Python as:

```python
2 ** 3
```

Output:

```text
8
```

The operator `**` means "raise to the power".

2.6.6 Remainder

The `%` operator returns the remainder after division.

```python
17 % 5
```

Output:

```text
2
```

The remainder operator is not central to semiconductor calculations, but it is an important Python arithmetic operator.

---

2.7 Order of Operations

Python follows the usual mathematical order of operations.

For example,

$$
2+3\times4
$$

is evaluated as

$$
2+(3\times4)=14.
$$

In Python:

```python
2 + 3 * 4
```

Output:

```text
14
```

Parentheses can be used to change the order.

```python
(2 + 3) * 4
```

Output:

```text
20
```

The general order is:

1. parentheses;
2. exponentiation;
3. multiplication and division;
4. addition and subtraction.

For scientific calculations, parentheses should be used whenever they make the intended calculation clearer.

---

2.8 Translating Mathematical Expressions into Python

A useful skill is learning to translate mathematical notation into Python syntax.

Consider:

$$
I=\frac{V}{R}.
$$

Python:

```python
I = V / R
```

Consider:

$$
P=VI.
$$

Python:

```python
P = V * I
```

Consider:

$$
E=mc^2.
$$

Python:

```python
E = m * c ** 2
```

Consider:

$$
x=\frac{a+b}{c}.
$$

Python:

```python
x = (a + b) / c
```

The translation is usually straightforward, but mathematical symbols must be replaced with Python syntax.

| Mathematics | Python |
| --- | --- |
| \(+\) | `+` |
| \(-\) | `-` |
| \(\times\) | `*` |
| \(\div\) | `/` |
| \(x^2\) | `x ** 2` |
| \(x^n\) | `x ** n` |
| \((a+b)/c\) | `(a + b) / c` |

---

2.9 Scientific Example: Ohm's Law

Ohm's law is

$$
V=IR.
$$

To calculate current,

$$
I=\frac{V}{R}.
$$

Suppose:

$$
V=5\,\mathrm{V}
$$

and

$$
R=220\,\Omega.
$$

Then

$$
I
=
\frac{5}{220}
=
0.022727\ldots\,\mathrm{A}.
$$

Python:

```python
V = 5
R = 220

I = V / R

print(I)
```

Output:

```text
0.022727272727272728
```

The result can be expressed approximately as:

$$
I\approx2.27\times10^{-2}\,\mathrm{A}
$$

or

$$
I\approx22.7\,\mathrm{mA}.
$$

The Python calculation is numerical; the conversion to milliamperes and the interpretation of the result require scientific understanding.

---

2.10 Calculating Different Quantities from the Same Relationship

A useful feature of mathematical models is that the same equation can often be rearranged to calculate different quantities.

Starting with Ohm's law:

$$
V=IR,
$$

we can calculate current:

$$
I=\frac{V}{R},
$$

resistance:

$$
R=\frac{V}{I},
$$

or voltage:

$$
V=IR.
$$

### Calculate Current

```python
V = 5
R = 100

I = V / R

print(I)
```

Output:

```text
0.05
```

### Calculate Resistance

```python
V = 5
I = 0.05

R = V / I

print(R)
```

Output:

```text
100.0
```

### Calculate Voltage

```python
I = 0.05
R = 100

V = I * R

print(V)
```

Output:

```text
5.0
```

The important programming lesson is that Python does not know Ohm's law. We provide the mathematical relationship through the code.

---

2.11 Scientific Example: Electrical Power

Electrical power is given by

$$
P=VI.
$$

Suppose:

$$
V=12\,\mathrm{V}
$$

and

$$
I=0.25\,\mathrm{A}.
$$

Then:

$$
P=(12)(0.25)=3\,\mathrm{W}.
$$

Python:

```python
V = 12
I = 0.25

P = V * I

print(P)
```

Output:

```text
3.0
```

Therefore,

$$
P=3.0\,\mathrm{W}.
$$

The same equation can be rearranged:

$$
V=\frac{P}{I}
$$

and

$$
I=\frac{P}{V}.
$$

This is an important habit in scientific programming:

> First establish the mathematical relationship. Then translate the relationship into Python.

---

2.12 Scientific Notation

Scientific and semiconductor calculations often involve very large or very small numbers.

For example:

$$
300\,000\,000
=
3\times10^8
$$

and

$$
0.000001
=
1\times10^{-6}.
$$

Python represents scientific notation using the letter `e`.

For example:

```python
speed_of_light = 3e8
```

represents

$$
3\times10^8.
$$

Similarly:

```python
current = 2.5e-3
```

represents

$$
2.5\times10^{-3}\,\mathrm{A}.
$$

This is particularly useful for semiconductor science because electrical and physical quantities may span many orders of magnitude.

Examples include:

$$
10^{-3},\quad
10^{-6},\quad
10^{-9},\quad
10^{-12}.
$$

In Python:

```python
milli = 1e-3
micro = 1e-6
nano = 1e-9
pico = 1e-12
```

---

2.13 Prefixes and Scientific Quantities

Common SI prefixes include:

| Prefix | Symbol | Factor |
| --- | --- | --- |
| kilo | k | \(10^3\) |
| mega | M | \(10^6\) |
| milli | m | \(10^{-3}\) |
| micro | \(\mu\) | \(10^{-6}\) |
| nano | n | \(10^{-9}\) |
| pico | p | \(10^{-12}\) |

For example,

$$
5\,\mathrm{mA}
=
5\times10^{-3}\,\mathrm{A}.
$$

Python:

```python
current = 5e-3
print(current)
```

Output:

```text
0.005
```

Similarly,

$$
20\,\mu\mathrm{A}
=
20\times10^{-6}\,\mathrm{A}.
$$

Python:

```python
current = 20e-6
print(current)
```

Output:

```text
2e-05
```

The output is Python's representation of the numerical value

$$
2\times10^{-5}\,\mathrm{A}.
$$

---

2.14 Working with Units

Python does not automatically attach units to ordinary numerical variables.

Consider:

```python
voltage = 5
resistance = 100
current = voltage / resistance
```

The calculation is numerically correct:

```text
0.05
```

But scientifically, we must interpret it as:

$$
I=0.05\,\mathrm{A}.
$$

The units come from the quantities used in the physical equation:

$$
\frac{\mathrm{V}}{\Omega}
=
\mathrm{A}.
$$

Therefore, scientific programming requires two levels of checking:

1. **Numerical check:** Did Python perform the arithmetic correctly?
2. **Scientific check:** Are the equation, units, and interpretation correct?

Both are necessary.

---

2.15 Temperature Conversion

Temperature conversion is a useful example because the mathematical relationship is simple but the units matter.

To convert Celsius temperature to Kelvin:

$$
T_K=T_C+273.15.
$$

Suppose:

$$
T_C=25\,^\circ\mathrm{C}.
$$

Then:

$$
T_K=25+273.15
=298.15\,\mathrm{K}.
$$

Python:

```python
T_C = 25

T_K = T_C + 273.15

print(T_K)
```

Output:

```text
298.15
```

Notice the variable names:

- `T_C` represents temperature in degrees Celsius;
- `T_K` represents temperature in kelvin.

Including the unit in the variable name can make scientific code easier to read.

---

2.16 Combining Multiple Calculations

Scientific calculations often involve more than one step.

Suppose the resistance of a device is

$$
R=100\,\Omega
$$

and the current is

$$
I=0.05\,\mathrm{A}.
$$

First calculate voltage:

$$
V=IR.
$$

Then calculate power:

$$
P=VI.
$$

Python:

```python
R = 100
I = 0.05

V = I * R
P = V * I

print(V)
print(P)
```

Output:

```text
5.0
0.25
```

Therefore,

$$
V=5\,\mathrm{V}
$$

and

$$
P=0.25\,\mathrm{W}.
$$

The variables allow the result of one calculation to become the input to another.

This is one of the fundamental ideas of programming:

$$
\boxed{
\text{Input}
\rightarrow
\text{Calculation}
\rightarrow
\text{Intermediate result}
\rightarrow
\text{Next calculation}
\rightarrow
\text{Final result}
}
$$

---

2.17 Rounding Numerical Results

Python may display more decimal places than are scientifically useful.

For example:

```python
V = 5
R = 220

I = V / R

print(I)
```

Output:

```text
0.022727272727272728
```

For many practical situations, we may want to display the result to a chosen number of decimal places.

Python's `round()` function can be used:

```python
print(round(I, 4))
```

Output:

```text
0.0227
```

The underlying numerical value and the displayed value are not necessarily the same.

Rounding is therefore mainly a matter of **presentation**, unless the rounded value is intentionally used in a later calculation.

> **Do not confuse display precision with measurement accuracy.**

A measurement recorded as \(5.00\,\mathrm{V}\) does not automatically mean that the physical measurement is accurate to \(0.01\,\mathrm{V}\).

---

2.18 Common Beginner Mistakes

### Mistake 1 — Using `^` for powers

A common mathematical habit is to write:

```python
2 ^ 3
```

for \(2^3\).

In Python, exponentiation is written using:

```python
2 ** 3
```

### Mistake 2 — Forgetting multiplication symbols

Mathematics allows:

$$
2V
$$

but Python requires:

```python
2 * V
```

### Mistake 3 — Confusing assignment and equality

In Python:

```python
V = 5
```

means "assign 5 to `V`".

It does not mean a mathematical equality statement in the same sense as a written equation.

### Mistake 4 — Ignoring units

A result such as

```text
0.05
```

is incomplete scientifically until we know what quantity it represents and what unit it carries.

### Mistake 5 — Excessive rounding

Rounding a result too early can affect later calculations.

### Mistake 6 — Using the wrong equation

Python will not warn you if you implement the wrong physical relationship.

---

2.19 Worked Example: Resistor Current and Power

A resistor has

$$
R=470\,\Omega
$$

and is connected to

$$
V=9\,\mathrm{V}.
$$

Calculate:

1. current;
2. electrical power.

### Step 1 — Mathematical model

Current:

$$
I=\frac{V}{R}.
$$

Power:

$$
P=VI.
$$

### Step 2 — Python implementation

```python
V = 9
R = 470

I = V / R
P = V * I

print(I)
print(P)
```

### Step 3 — Output

```text
0.019148936170212766
0.17234042553191488
```

### Step 4 — Scientific interpretation

The current is approximately

$$
I\approx0.01915\,\mathrm{A}
$$

or

$$
I\approx19.15\,\mathrm{mA}.
$$

The power is approximately

$$
P\approx0.1723\,\mathrm{W}.
$$

Thus, under the stated conditions, the resistor carries approximately \(19.15\,\mathrm{mA}\) and dissipates approximately \(0.1723\,\mathrm{W}\).

---

2.20 Worked Example: A Semiconductor-Scale Current

Suppose a semiconductor device carries a current of

$$
I=250\,\mu\mathrm{A}.
$$

Represent this current in amperes using Python.

### Mathematical representation

$$
250\,\mu\mathrm{A}
=
250\times10^{-6}\,\mathrm{A}.
$$

### Python

```python
I = 250e-6

print(I)
```

Output:

```text
0.00025
```

Therefore,

$$
I=2.5\times10^{-4}\,\mathrm{A}.
$$

The `e` notation provides a convenient way to represent values involving powers of ten.

---

2.21 Worked Example: Evaluating a Mathematical Expression

Consider:

$$
y=\frac{2x^2+3x+1}{x+1}.
$$

For

$$
x=2,
$$

the value is

$$
y=
\frac{2(2)^2+3(2)+1}{2+1}
=
\frac{8+6+1}{3}
=
5.
$$

Python:

```python
x = 2

y = (2 * x ** 2 + 3 * x + 1) / (x + 1)

print(y)
```

Output:

```text
5.0
```

The example illustrates an important skill:

> Translate the mathematical structure into Python carefully, using parentheses where necessary.

---

2.22 Exercises

### Exercise 1 — Basic Arithmetic

Evaluate the following expressions using Python:

$$
15+27
$$

$$
100-37
$$

$$
12\times8
$$

$$
144/12.
$$

---

### Exercise 2 — Powers

Use Python to calculate:

$$
2^5,\qquad
10^3,\qquad
10^{-6}.
$$

---

### Exercise 3 — Ohm's Law

A resistor has

$$
R=330\,\Omega
$$

and a voltage of

$$
V=3.3\,\mathrm{V}.
$$

Calculate the current in amperes and milliamperes.

---

### Exercise 4 — Electrical Power

A device operates at

$$
V=5\,\mathrm{V}
$$

and draws

$$
I=20\,\mathrm{mA}.
$$

Calculate its power in watts.

Remember to convert:

$$
20\,\mathrm{mA}=20\times10^{-3}\,\mathrm{A}.
$$

---

### Exercise 5 — Temperature

Convert the following temperatures from Celsius to Kelvin:

$$
0\,^\circ\mathrm{C},\qquad
25\,^\circ\mathrm{C},\qquad
100\,^\circ\mathrm{C}.
$$

Use

$$
T_K=T_C+273.15.
$$

---

### Exercise 6 — Scientific Notation

Write the following quantities using Python scientific notation:

$$
3\times10^{-3},
\qquad
5\times10^{-6},
\qquad
7.2\times10^{-9},
\qquad
1.5\times10^6.
$$

---

### Exercise 7 — Scientific Calculation

A resistor has

$$
R=1\,\mathrm{k}\Omega
$$

and carries a current of

$$
I=2\,\mathrm{mA}.
$$

Calculate the voltage and power.

Use:

$$
V=IR
$$

and

$$
P=VI.
$$

Be careful with unit conversion.

---

2.23 Think and Apply

A student writes:

```python
V = 5
R = 100
I = R / V
```

The program runs without producing an error.

1. Is the Python syntax valid?
2. Is the scientific calculation correct for Ohm's law?
3. What should the expression for current be?
4. Why can a program execute successfully and still produce a scientifically incorrect result?

---

2.24 Chapter Summary

In this chapter, we learned how to use Python as a scientific calculator.

The key ideas are:

- Python can evaluate mathematical expressions directly.
- Variables allow numerical values to be stored and reused.
- Arithmetic operators translate common mathematical operations into Python syntax.
- Parentheses can be used to control the order of operations.
- Scientific notation can be represented using `e` notation.
- Python can implement equations used in electronics and semiconductor science.
- Physical units must be tracked by the scientist.
- Numerical output must be interpreted scientifically.
- A syntactically correct program can still implement an incorrect scientific model.
- Mathematical formulation should come before Python implementation.

The next chapter introduces **data types, input, and output**, allowing programs to work with different kinds of information and interact with users.
